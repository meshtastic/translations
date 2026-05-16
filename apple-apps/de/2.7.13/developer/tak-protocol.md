
# TAK-Protokoll
Die App implementiert drei CoT-Drahtformate (Cursor-on-Target) über LoRa: zwei Legacy-V1-Pfade und einen V2-Pfad, der die [TAKPacket-SDK](https://github.com/meshtastic/TAKPacket-SDK) für zstd-dictionary-Komprimierung und reichhaltigere typisierte Nutzlasten verwendet. Diese Seite dokumentiert die Wahl des Formats, den Empfangsversand und die unterstützende Infrastruktur.
## Drahtformate
| Format | Hafen | Zuladung | Verwendet, wenn |
|--------|------|---------|-----------|
| V1 ATAK_STECKER | 72 | Bare `TAKPacket` protobuf (nur PLI / GeoChat) | Verbundene Funk-Firmware < 2.8.0 |
| V1 ATAK_FORDER | 257 | Zlib-komprimiertes CoT-XML, optional Fountain (LT)-codiert für Multi-Packet-Payloads | Verbundene Funk-Firmware < 2.8.0, CoT-Typ außer PLI / GeoChat (nur Apple-zu-Apple) |
| V2 ATAK_STECKER_V2 | 78 | `TAKPacketV2` protobuf, komprimiert mit dem TAKPacket-SDK zstd Wörterbuch | Verbundene Funk-Firmware ≥ 2,8.0 |

V2 enthält das vollständige typisierte CoT-Vokabular: PLI, GeoChat, Formen, Marker, Routen, Casevac, Notfall und Aufgabe. V1 ATAK_PLUGIN enthält nur PLI und GeoChat; alles andere fällt auf den V1 ATAK_FORWARDER-Pfad zurück.
## Per-Send-Gabel
`TAKMeshtasticBridge.sendToMesh(_:clientInfo:)`Wählt das Format bei jeder Sendung basierend auf`AccessoryManager.supportsTAKv2`, Die die Firmware-Version des angeschlossenen Radios überprüft:
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

Der Fork ist pro Send (nicht pro Sitzung), so dass ein Radio, das in der Mitte der Sitzung aufrüstet, sofort zu V2 springt.
## Versand erhalten
`AccessoryManager.swift`Der erschöpfende Portnum-Switch sendet eingehende TAK-Pakete an Handler in`AccessoryManager+TAK.swift`:
| Portnum | Anwender | Verhalten |
|---------|---------|----------|
| `.atakPlugin` (72) | `handleATAKPluginPacket(_:)` | Entschlüsseln Sie bare `TAKPacket` protobuf; konvertieren Sie PLI / GeoChat in `CoTMessage`; leiten Sie über `TAKServerManager.shared.broadcast(_:)` an TAK-Clients weiter. |
| `.atakPluginV2` (78) | `handleATAKPluginV2Packet(_:)` | Zstd-dekomprimieren mit `TakCompressor`; CoT XML mit `CotXmlBuilder` neu erstellen; XML-Prolog und Inter-Tag-Leerzeichen entfernen; Roh-XML über `broadcastRawXml(_:)` weiterleiten, damit Formdetails (`<link point>` Scheitelpunkte, Farben, Strich) überleben. Route CoT (`b-m-r`) löst den Nebeneffekt [Routendatenpaket](#route-data-packages) aus. |
| `.atakForwarder` (257) | `handleATAKForwarderPacket(_:)` | Übergabe an `GenericCoTHandler.handleIncomingForwarderPacket(_:)`, das Fountain-Fragmente wieder zusammensetzt und das resultierende CoT-XML vor der Übertragung zlib-dekomprimiert. |

## TAKPaket-SDK
Das [TAKPacket-SDK](https://github.com/meshtastic/TAKPacket-SDK) Swift-Paket ist angeheftet`Meshtastic.xcworkspace/.../Package.resolved`. Die Brücke verwendet drei APIs:
-`MeshtasticTAK.CotXmlParser().parse(_:)`— Parst CoT XML in ein`TAKPacketV2`Protobuf.`throws`; Anrufer müssen`try`.-`MeshtasticTAK.TakCompressor().compressWithRemarksFallback(_:maxWireBytes:)`- Wählt das zstd-Wörterbuch aus, das am besten zur Nutzlast passt, versucht Komprimierung und versucht bei Überlauf erneut mit`<remarks>`Ausgezogen. Rücksendungen`nil`Wenn die Nutzlast auch ohne Anmerkungen zu groß ist;`sendCoTToMeshV2`Übersetzt die`nil`In`AccessoryError.ioFailed(...)`Also der Anrufer`do/catch`Behandelt den Drop nicht als erfolgreiches Senden.-`MeshtasticTAK.CotXmlBuilder().build(_:)`— Hin- und Rückfahrt`TAKPacketV2`Zurück zu CoT XML für die Weiterleitung an TAK-Clients.
Die Draht-MTU-Konstante`maxWirePayloadBytes = 225`Spiegelt das LoRa-Frame-Budget nach dem Meshtastic-Umschlag wider.
## Identitätsadministrator
`TAKIdentitySection`(Eingebettet in`TAKServerConfig`) Liest`node.takConfig`Für die aktuellen Team- und Rollenwerte. Wenn der Knoten keine TAK-Konfiguration zwischengespeichert hat,`requestTakConfigIfNeeded()`Löst eine Admin-Anfrage über`AccessoryManager.requestTAKModuleConfig(fromUser:toUser:)`So sehen Erstbenutzer keinen Perma-Spinner.
Speichern der Identitätssendungen`AccessoryManager.saveTAKModuleConfig(config:fromUser:toUser:)`, Die ein`ModuleConfig.TAKConfig`Innerhalb eines`AdminMessage`Und versendet es an den Admin-Port.
## Offline-Warteschlange
`TAKServerManager`Puffert ausgehende CoT für die Lieferung, wenn sich ein TAK-Client wieder verbindet:
```swift
private enum QueuedPayload {
    case message(CoTMessage)
    case rawXml(String)
}
```

`broadcast(_:)`In der Schlangen`.message`Nutzlasten;`broadcastRawXml(_:)`In der Schlangen`.rawXml`So behalten V2-Formen / Routen / Marker ihre Detailelemente. Die Warteschlange hat eine 5-minütige TTL und eine 50-Eintragsobergrenze.`drainOfflineQueue()`Sendet den richtigen Pfad pro Nutzlastvariante, wenn ein Client (wieder) eine Verbindung herstellen kann.
## Routendatenpakete
`RouteDataPackageGenerator`(In`Meshtastic/Helpers/TAK/`) Wandelt die Route CoT um (`b-m-r`) In KML-inside-zip ATAK-Datenpakete, die der Benutzer in iTAK sideloaden kann (das die Route, die CoT über seine TCP-Streaming-Verbindung erhalten hat, stillschweigend ignoriert).
Die Pipeline:
1.`generateKml(routeXml:)`Extrakte`<event uid>`,`<contact callsign>`, Und jeder`<link point="lat,lon,hae">`Wegpunkt über`attributeValue(in:on:named:)`, Die sowohl Attribute mit einfachen als auch mit doppelten Anführungszeichen unterstützt.2.`sanitizeForFilename(_:)`Streifen Pfadtrennzeichen, Steuerzeichen und`..`Sequenzen von der Route-UID, so dass es sicher ist, in Dateinamen und dem temporären Verzeichnispfad zu verwenden.`escapeXml(_:)`Entweicht separat dem Wert vor der Interpolation in das Manifest`value="..."`Attribut.3.`generateDataPackage(routeXml:)`Schreibt die KML und`manifest.xml`Zu einem temporären Verzeichnis und zippt sie mit`NSFileCoordinator(readingItemAt:options:.forUploading)`.4.`saveToDocuments(fileName:zipData:)`Schreibt den Reißverschluss an`Documents/TAK Routes/<sanitizedUid>.zip`(Erstelten des Verzeichnisss bei der ersten Verwendung).5.`AccessoryManager+TAK.handleATAKPluginV2Packet(_:)`Beiträge a`Notification`Mit dem Titel **Route Empfangen** mit dem Routen-Aufrufzeichen als Untertitel und Hauptteil **Gespeichert in Dateien → Meshtastic → TAK-Routen. Öffnen Sie in iTAK zum Importieren. **
## Fähigkeiten
`AccessoryManager.supportsTAKv2`Ist das kanonische Tor:
```swift
var supportsTAKv2: Bool { checkIsVersionSupported(forVersion: "2.8.0") }
```

Verwenden Sie diese Eigenschaft (anstatt die Firmware-Version inline zu parsen), wo immer eine V1/V2-Entscheidung erforderlich ist. Zukünftige TAK SDK-Funktionen, die eine höhere Firmware-Version erfordern, sollten eine Geschwistereigenschaft mit einem klaren Cut-off hinzufügen, damit die Bridge deklarativ bleibt.
## Verwandte Dateien
-`Meshtastic/Helpers/TAK/TAKMeshtasticBridge.swift`— V1/V2 Gabelung`sendToMesh`.-`Meshtastic/Accessory/Accessory Manager/AccessoryManager+TAK.swift`— Handler senden und empfangen.-`Meshtastic/Helpers/TAK/TAKServerManager.swift`— TCP-Server, Offline-Warteschlange, Zertifikatsverwaltung.-`Meshtastic/Helpers/TAK/GenericCoTHandler.swift`— V1 ATAK_FORWARDER Klassifizierung und Brunnenzusammenbau.-`Meshtastic/Helpers/TAK/RouteDataPackageGenerator.swift`— KML-Datenpaket-Autor.-`Meshtastic/Views/Settings/TAKServerConfig.swift`— Kombinierter TAK-Server-Einstellungensbildschirm mit dem eingebetteten`TAKIdentitySection`.
Siehe auch [Transport Layer](transport.html) für die AccessoryManager-Erweiterungskarte.
