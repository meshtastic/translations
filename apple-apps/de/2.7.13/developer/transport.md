
# Transportschicht

`AccessoryManager`Abstrahiert BLE, TCP/IP und serielle Transporte hinter einer einzigen Schnittstelle. Ansichten und Dienste interagieren nur mit`AccessoryManager`- Niemals mit Transportimplementierungen direkt.

## Transportimplementierungen

Transport leben in`Meshtastic/Accessory/Transports/`:

| Dossier | Kommunikationsprotokoll | Notizen |
|------|----------|-------|
| `BLETransport.swift` | CoreBluetooth | Standard-BLE-Anschluss an Funkgeräte |
| `TCPTransport.swift` | Netzwerk.Framework | Wi-Fi / TCP/IP zu Funkgeräten mit Netzwerk |
| `Serieller Transport.schnell` | IOKit-Serie | Nur macOS; USB-Serienadapter |

Jeder Transport entspricht einem`MeshTransport`Protokoll, das entlarvt`connect()`,`disconnect()`,`send(data:)`, Und ein`received`Verleger.

## AccessoryManager-Erweiterungskarte

| Dateinamenserweiterung | Schlüsselmethoden |
|-----------|------------|
| `+Entdeckung` | `startScanning()`, `stopScanning()`, `Peripheriegerät(_:didDiscover:)` |
| `+Verbinden` | `connect(peripheral:)`, `disconnect()`, `centralManager(_:didConnect:)` |
| `+ToRadio` | `sendPacket(_:)`, `sendWantConfig()`, `sendWaypoint(_:)` |
| `+Aus dem Radio` | `HandleFromRadio(_:)`, `HandleMeshPacket(_:)` |
| `+Position` | `startLocationUpdates()`, `sendPosition(_:)` |
| `+MQTT` | `connectMQTT()`, `publishPacket(_:)`, `mqttClient(_:didReceiveMessage:)` |
| `+TAK` | `handleATAKPluginPacket(_:)`, `handleATAKPluginV2Packet(_:)`, `handleATAKForwarderPacket(_:)`, `sendTAKPacket(_:channel:)`, `sendTAKV2Packet(_:channel:)`, `sendCoTToMeshV2(_:channel:channel:)`. Siehe [TAK-Protokoll](tak-protocol.html) für Details zum V1/V2-Drahtformat. |

## Paketfluss (eingehend)

```
Radio (BLE/TCP/Serial)
  → Transport.received publisher
  → AccessoryManager+FromRadio.handleFromRadio(_:)
  → Decode protobuf (MeshtasticProtobufs)
  → Route by packet type:
      MeshPacket  → handleMeshPacket(_:)
      NodeInfo    → updateNodeInfo(_:)
      MyNodeInfo  → updateMyNodeInfo(_:)
      Config      → updateConfig(_:)
      ...
  → Write to SwiftData via MeshPackets @ModelActor
  → Publish changes via @Published properties (UI updates)
```

## Paketfluss (ausgehend)

```
View / Service
  → AccessoryManager+ToRadio.sendPacket(_:)
  → Encode to protobuf (ToRadio wrapper)
  → Transport.send(data:)
  → Radio
```

## Hinzufügen eines neuen Pakettyps

1. Fügen Sie die protobuf-Definition in der`protobufs/`Untermodul.
2. Lauf`./scripts/gen_protos.sh`.
3. Fügen Sie einen Decode/Dispatch-Fall in`AccessoryManager+FromRadio.handleFromRadio(_:)`.
4. Fügen Sie eine Send-Methode in`AccessoryManager+ToRadio.swift`.
5. Fügen Sie eine Modelleigenschaft oder eine SwiftData-Entität hinzu, wenn die Daten beibehalten werden müssen.
6. Schreiben Sie Einheitstests gegen die Encode/Decod-Roundtrip.

## Hinweise zur Parallelität

`AccessoryManager`Ist nicht`@MainActor`. Sein`@Published`Eigenschaften werden von SwiftUI-Ansichten auf den Hauptakteur beobachtet. Nutzung`await MainActor.run { }`Bei der Aktualisierung veröffentlichter Eigenschaften aus Hintergrundaufgaben oder CoreBluetooth Delegaten-Callbacks.

Hintergrundpersistenz schreibt muss durch die`MeshPackets` `@ModelActor`, Nicht der Haupt`ModelContext`.

