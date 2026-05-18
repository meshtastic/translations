
# Protocole TAK

L'application implémente trois formats de fil CoT (Cursor-on-Target) sur LoRa : deux chemins V1 hérités et un chemin V2 qui utilise le [TAKPacket-SDK](https://github.com/meshtastic/TAKPacket-SDK) pour la compression zstd-dictionnaire et les charges utiles typées plus riches. Cette page documente le choix du format, l'expédition de réception et l'infrastructure de support.

## Formats de fil

| Format | Port | Charge utile | Utilisé quand |
|--------|------|---------|-----------|
| V1 ATAK_PLUGIN | 72 | Nu`TAKPacket`Protobuf (PLI / GeoChat uniquement) | Firmware radio connecté < 2.8.0 |
| V1 ATAK_APPROVISIONNEUR | 257 | XML CoT compressé par zlib, en option Source (LT) codé pour les charges utiles multi-paquets | Micrologiciel radio connecté < 2.8.0, type CoT autre que PLI / GeoChat (Apple à Apple uniquement) |
| V2 ATAK_PLUGIN_V2 | 78 | `TAKPacketV2`Protobuf, compressé avec le dictionnaire TAKPacket-SDK zstd | Firmware radio connecté ≥ 2.8.0 |

La V2 contient le vocabulaire CoT typé complet : PLI, GeoChat, formes, marqueurs, itinéraires, casevac, urgence et tâche. V1 ATAK_PLUGIN ne transporte que PLI et GeoChat ; tout le reste revient au chemin V1 ATAK_FORWARDER.

## Fourchette par ensoi

`TAKMeshtasticBridge.sendToMesh(_:clientInfo:)`Choisit le format sur chaque envoi en fonction de`AccessoryManager.supportsTAKv2`, Qui vérifie la version du micrologiciel de la radio connectée :

```swift
if accessoryManager.supportsTAKv2 {
    // V2: SDK-driven path
    let parser = MeshtasticTAK.CotXmlParser()
    let packet = try parser.parse(strippedXml)
    let compressor = MeshtasticTAK.TakCompressor()
    // `compressWithRemarksFallback` returns `Data?` — `nil` means the
    // payload is still over the LoRa MTU even after `<remarks>` are
    // stripped. The real `sendCoTToMeshV2` translates that into a
    // thrown `AccessoryError.ioFailed(...)` so the caller's `do/catch`
    // doesn't treat the silent drop as a successful send.
    guard let wire = try compressor.compressWithRemarksFallback(packet, maxWireBytes: 225) else {
        throw AccessoryError.ioFailed("TAK V2 payload exceeds LoRa wire size limit")
    }
    try await sendTAKV2Packet(wire, channel: channel)
} else {
    // V1: classify, then dispatch
    switch GenericCoTHandler.shared.classifySendMethod(for: cot) {
    case .takPacketPLI, .takPacketChat:
        let pkt = convertToTAKPacket(cot: cot)
        try await sendTAKPacket(pkt, channel: channel)
    case .exiDirect, .exiFountain:
        try await GenericCoTHandler.shared.sendGenericCoT(cot, channel: channel)
    }
}
```

Le fork est par envoi (pas par session), de sorte qu'une radio qui met à niveau en milieu de session passe immédiatement à la V2.

## Recevoir l'envoi

`AccessoryManager.swift`Le commutateur portnum exhaustif envoie les paquets TAK entrants aux gestionnaires en`AccessoryManager+TAK.swift`:

| Portnum | Maître-chien | Comportement |
|---------|---------|----------|
| `.atakPlugin`(72) | `handleATAKPluginPacket(_:)` | Décoder nu`TAKPacket`Protobuf ; convertir PLI / GeoChat en`CoTMessage`; Transmettre aux clients TAK via`TAKServerManager.shared.broadcast(_:)`. |
| `.atakPluginV2`(78) | `handleATAKPluginV2Packet(_:)` | Zstd-décompresser avec`TakCompressor`; Reconstruire CoT XML avec`CotXmlBuilder`; Retirer le prologue XML et les espaces blancs inter-balises ; transmettre XML brut via`broadcastRawXml(_:)`Donc détail de la forme (`<link point>`Sommets, couleurs, trait) survit. Route CoT (`b-m-r`) Déclenche l'effet secondaire [paquet de données de route](#route-data-packages). |
| `.atakForwarder`(257) | `handleATAKForwarderPacket(_:)` | Remettre à`GenericCoTHandler.handleIncomingForwarderPacket(_:)`, Qui réassemble les fragments de fontaine et zlib-décompresse le CoT XML résultant avant la diffusion. |

## Paquet TAK-SDK

Le paquet Swift [TAKPacket-SDK](https://github.com/meshtastic/TAKPacket-SDK) Swift est épinglé dans`Meshtastic.xcworkspace/.../Package.resolved`. Le pont utilise trois API :

-`MeshtasticTAK.CotXmlParser().parse(_:)`— Analyse CoT XML dans un`TAKPacketV2`Protobuf.`throws`; Les appelants doivent`try`.
-`MeshtasticTAK.TakCompressor().compressWithRemarksFallback(_:maxWireBytes:)`- Choisit le dictionnaire zstd le mieux adapté à la charge utile, tente la compression et sur les tentatives de débordement avec`<remarks>`Dépouillé. Retours`nil`Si la charge utile est trop importante même sans remarques ;`sendCoTToMeshV2`Traduit le`nil`Dans`AccessoryError.ioFailed(...)`Donc l'appelant`do/catch`Ne traite pas la baisse comme un envoi réussi.
-`MeshtasticTAK.CotXmlBuilder().build(_:)`— Aller-retour`TAKPacketV2`Retour à CoT XML pour la transmission aux clients TAK.

La constante MTU du fil`maxWirePayloadBytes = 225`Reflète le budget du cadre LoRa après l'enveloppe Meshtastic.

## Administrateur d'identité

`TAKIdentitySection`(Intégré dans`TAKServerConfig`) Lit`node.takConfig`Pour les valeurs actuelles de l'équipe et du rôle. Lorsque le nœud n'a pas de configuration TAK en cache,`requestTakConfigIfNeeded()`Déclenche une demande d'administrateur via`AccessoryManager.requestTAKModuleConfig(fromUser:toUser:)`Donc les nouveaux utilisateurs ne voient pas de perma-spinner.

Enregistrement des envois d'identité`AccessoryManager.saveTAKModuleConfig(config:fromUser:toUser:)`, Qui empaquete un`ModuleConfig.TAKConfig`À l'intérieur d'un`AdminMessage`Et l'expédie sur le port d'administration.

## File d'attente hors ligne

`TAKServerManager`Tamponne le CoT sortant pour la livraison lorsqu'un client TAK se reconnecte :

```swift
private enum QueuedPayload {
    case message(CoTMessage)
    case rawXml(String)
}
```

`broadcast(_:)`Files d'attente`.message`Charges utiles ;`broadcastRawXml(_:)`Files d'attente`.rawXml`De sorte que les formes / itinéraires / marqueurs V2 conservent leurs éléments de détail. La file d'attente a un TTL de 5 minutes et un plafond de 50 entrées.`drainOfflineQueue()`Envoie le bon chemin par variante de charge utile lorsqu'un client se (re)connecte.

## Paquets de données d'itinéraire

`RouteDataPackageGenerator`(En`Meshtastic/Helpers/TAK/`) Convertit la route CoT (`b-m-r`) Dans les paquets de données ATAK KML-inside-zip que l'utilisateur peut charger dans iTAK (qui ignore silencieusement l'itinéraire que CoT a reçu sur sa connexion de streaming TCP).

Le pipeline :

1.`generateKml(routeXml:)`Extraits`<event uid>`,`<contact callsign>`, Et chaque`<link point="lat,lon,hae">`Waypoint via`attributeValue(in:on:named:)`, Qui prend en charge les attributs entre guillemets simples et doubles.
2.`sanitizeForFilename(_:)`Séparateurs de chemins de bandes, caractères de contrôle et`..`Séquences de l'UID de la route afin qu'il soit sûr d'utiliser dans les noms de fichiers et le chemin du répertoire temporaire.`escapeXml(_:)`Séparément, la valeur s'échappe avant l'interpolation dans le manifeste`value="..."`Attribut.
3.`generateDataPackage(routeXml:)`Écrit le KML et`manifest.xml`À un répertoire temporaire et les compresse avec`NSFileCoordinator(readingItemAt:options:.forUploading)`.
4.`saveToDocuments(fileName:zipData:)`Écrit le zip à`Documents/TAK Routes/<sanitizedUid>.zip`(Création du répertoire lors de la première utilisation).
5.`AccessoryManager+TAK.handleATAKPluginV2Packet(_:)`Publie un`Notification`Intitulé **Route Received** avec l'insigne d'appel de l'itinéraire comme sous-titre et corps **Enregistré dans Fichiers → Meshtastic → TAK Routes. Ouvrir dans iTAK pour importer. **

## Capacités

`AccessoryManager.supportsTAKv2`Est la porte canonique :

```swift
var supportsTAKv2: Bool { checkIsVersionSupported(forVersion: "2.8.0") }
```

Utilisez cette propriété (plutôt que d'analyser la version du firmware en ligne) partout où une décision V1/V2 est nécessaire. Les futures fonctionnalités du SDK TAK qui nécessitent une version supérieure du micrologiciel devraient ajouter une propriété sœur avec une coupure claire afin que le pont reste déclaratif.

## Fichiers connexes

-`Meshtastic/Helpers/TAK/TAKMeshtasticBridge.swift`— Fourche V1/V2`sendToMesh`.
-`Meshtastic/Accessory/Accessory Manager/AccessoryManager+TAK.swift`- Envoyer et recevoir des gestionnaires.
-`Meshtastic/Helpers/TAK/TAKServerManager.swift`- Serveur TCP, file d'attente hors ligne, gestion des certificats.
-`Meshtastic/Helpers/TAK/GenericCoTHandler.swift`- Classification V1 ATAK_FORWARDER et réassemblage de la fontaine.
-`Meshtastic/Helpers/TAK/RouteDataPackageGenerator.swift`- Rédacteur de paquets de données KML.
-`Meshtastic/Views/Settings/TAKServerConfig.swift`- Écran de paramètres du serveur TAK combiné avec l'intégration`TAKIdentitySection`.

Voir aussi [Transport Layer](transport.html) pour la carte d'extension AccessoryManager.

