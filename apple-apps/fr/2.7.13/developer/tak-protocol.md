
# Protocole TAK
L'application implémente trois formats de fil CoT (Cursor-on-Target) sur LoRa : deux chemins V1 hérités et un chemin V2 qui utilise le [TAKPacket-SDK](https://github.com/meshtastic/TAKPacket-SDK) pour la compression zstd-dictionnaire et les charges utiles typées plus riches. Cette page documente le choix du format, l'expédition de réception et l'infrastructure de support.
## Formats de fil
| Format | Port | Charge utile | Utilisé quand |
|--------|------|---------|-----------|
| V1 ATAK_PLUGIN | 72 | Protobuf nu `TAKPacket` (PLI / GeoChat uniquement) | Firmware radio connecté < 2.8.0 |
| V1 ATAK_APPROVISIONNEUR | 257 | XML CoT compressé par zlib, en option Source (LT) codé pour les charges utiles multi-paquets | Micrologiciel radio connecté < 2.8.0, type CoT autre que PLI / GeoChat (Apple à Apple uniquement) |
| V2 ATAK_PLUGIN_V2 | 78 | `TAKPacketV2` protobuf, compressé avec le dictionnaire TAKPacket-SDK zstd | Firmware radio connecté ≥ 2.8.0 |

La V2 contient le vocabulaire CoT typé complet : PLI, GeoChat, formes, marqueurs, itinéraires, casevac, urgence et tâche. V1 ATAK_PLUGIN ne transporte que PLI et GeoChat ; tout le reste revient au chemin V1 ATAK_FORWARDER.
## Fourche par envoyer
`TAKMeshtasticBridge.sendToMesh(_:clientInfo:)` choisit le format sur chaque envoi en fonction de `AccessoryManager.supportsTAKv2`, qui vérifie la version du micrologiciel de la radio connectée :
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
## Recevoir l'expédition
Le commutateur portnum exhaustif de `AccessoryManager.swift` envoie les paquets TAK entrants aux gestionnaires dans `AccessoryManager+TAK.swift` :
| Portnum | Maître-chien | Comportement |
|---------|---------|----------|
| `.atakPlugin` (72) | `handleATAKPluginPacket(_:)` | Décodez le protobuf `TAKPacket` nu ; convertissez PLI / GeoChat en `CoTMessage` ; transférez aux clients TAK via `TAKServerManager.shared.broadcast(_:)`. |
| `.atakPluginV2` (78) | `handleATAKPluginV2Packet(_:)` | Zstd-décompresser avec `TakCompressor` ; reconstruire CoT XML avec `CotXmlBuilder` ; retirer le prologue XML et l'espace blanc inter-tag ; avancer XML brut via `broadcastRawXml(_:)` afin que le détail de la forme (`<link point>` sommets, couleurs, trait) survive. Route CoT (`b-m-r`) déclenche l'effet secondaire [paquet de données de route](#route-data-packages). |
| `.atakforwarder` (257) | `handleATAKForwarderPacket(_:)` | Remettre à `GenericCoTHandler.handleIncomingForwarderPacket(_:)`, qui réassemble les fragments de Fountain et zlib-décompresse le CoT XML résultant avant la diffusion. |

## Paquet-SDK TAK
Le paquet Swift [TAKPacket-SDK](https://github.com/meshtastic/TAKPacket-SDK) Swift est épinglé dans `Meshtastic.xcworkspace/... /Paquet.résolu`. Le pont utilise trois API :
- `MeshtasticTAK.CotXmlParser().parse(_:)` — analyse CoT XML dans un protobuf `TAKPacketV2`. `throws` ; les appelants doivent `essayer`.- `MeshtasticTAK.TakCompressor().compressWithRemarksFallback(_:maxWireBytes:)` - choisit le dictionnaire zstd le mieux adapté à la charge utile, tente la compression, et sur le débordement, tente à nouveau avec `<remarks>` dépouillé. Renvoie `nil` si la charge utile est trop importante même sans remarques ; `sendCoTToMeshV2` traduit le `nil` en `AccessoryError.ioFailed(...)` de sorte que le `do/catch` de l'appelant ne traite pas la chute comme un envoi réussi.- `MeshtasticTAK.CotXmlBuilder().build(_:)` - aller-retour `TAKPacketV2` vers CoT XML pour la transmission aux clients TAK.
La constante MTU du fil `maxWirePayloadBytes = 225` reflète le budget du cadre LoRa après l'enveloppe Meshtastic.
## Administrateur d'identité
`TAKIdentitySection` (intégré dans `TAKServerConfig`) indique `node.takConfig` pour les valeurs actuelles de l'équipe et du rôle. Lorsque le nœud n'a pas de configuration TAK mise en cache, `requestTakConfigIfNeeded()` déclenche une demande d'administrateur via `AccessoryManager.requestTAKModuleConfig(fromUser:toUser:)` afin que les nouveaux utilisateurs ne voient pas de perma-spinner.
L'enregistrement de l'identité envoie `AccessoryManager.saveTAKModuleConfig(config:fromUser:toUser:)`, qui emballe un `ModuleConfig.TAKConfig` à l'intérieur d'un `AdminMessage` et l'expédie sur le port d'administration.
## File d'attente hors ligne
`TAKServerManager` met en mémoire tampon le CoT sortant pour la livraison lorsqu'un client TAK se reconnecte :
```swift
private enum QueuedPayload {
    case message(CoTMessage)
    case rawXml(String)
}
```

`broadcast(_:)` met en queue `.message` les charges utiles ; `broadcastRawXml(_:)` met en queue `.rawXml` afin que les formes / routes / marqueurs V2 conservent leurs éléments de détail. La file d'attente a un TTL de 5 minutes et un plafond de 50 entrées. `drainOfflineQueue()` envoie le bon chemin par variante de charge utile lorsqu'un client se (re)connecte.
## Paquets de données d'itinéraire
`RouteDataPackageGenerator` (dans `Meshtastic/Helpers/TAK/`) convertit la route CoT (`b-m-r`) en paquets de données ATAK KML-inside-zip que l'utilisateur peut charger dans iTAK (qui ignore silencieusement la route CoT reçue sur sa connexion de streaming TCP).
Le pipeline :
1. `generateKml(routeXml:)` extrait `<event uid>`, `<contact callsign>`, et chaque `<link point="lat,lon,hae">` waypoint via `attributeValue(in:on:on:named:)`, qui prend en charge les attributs de guillemets simples et doubles.2. `sanitizeForFilename(_:)` supprime les séparateurs de chemin, les caractères de contrôle et les séquences `..` de l'UID de la route afin qu'il soit sûr de l'utiliser dans les noms de fichiers et le chemin du répertoire temporaire. `escapeXml(_:)` échappe séparément de la valeur avant l'interpolation dans l'attribut `value="..."` du manifeste.3. `generateDataPackage(routeXml:)` écrit le KML et `manifest.xml` dans un répertoire temporaire et les compresse avec `NSFileCoordinator(readingItemAt:options:.forUploading)`.4. `saveToDocuments(fileName:zipData:)` écrit le zip à `Documents/TAK Routes/<sanitizedUid>.zip` (création du répertoire lors de la première utilisation).5. `AccessoryManager+TAK.handleATAKPluginV2Packet(_:)` publie une `Notification` intitulée **Route Received** avec l'insigne d'appel de l'itinéraire comme sous-titre et corps **Enregistré dans les fichiers → Meshtastic → TAK Routes. Ouvrir dans iTAK pour importer. **
## Capacités
`AccessoryManager.supportsTAKv2` est la porte canonique :
```swift
var supportsTAKv2: Bool { checkIsVersionSupported(forVersion: "2.8.0") }
```

Utilisez cette propriété (plutôt que d'analyser la version du firmware en ligne) partout où une décision V1/V2 est nécessaire. Les futures fonctionnalités du SDK TAK qui nécessitent une version supérieure du micrologiciel devraient ajouter une propriété sœur avec une coupure claire afin que le pont reste déclaratif.
## Fichiers connexes
- `Meshtastic/Helpers/TAK/TAKMeshtasticBridge.swift` - Fourche V1/V2 dans `sendToMesh`.- `Meshtastic/Accessory/Accessory Manager/AccessoryManager+TAK.swift` - envoyer et recevoir des gestionnaires.- `Meshtastic/Helpers/TAK/TAKServerManager.swift` - serveur TCP, file d'attente hors ligne, gestion des certificats.- `Meshtastic/Helpers/TAK/GenericCoTHandler.swift` - Classification V1 ATAK_FORWARDER et réassemblage de la fontaine.- `Meshtastic/Helpers/TAK/RouteDataPackageGenerator.swift` - Rédacteur de paquets de données KML.- `Meshtastic/Views/Settings/TAKServerConfig.swift` - écran des paramètres du serveur TAK combiné avec le `TAKIdentitySection` intégré.
Voir aussi [Transport Layer](transport.html) pour la carte d'extension AccessoryManager.
