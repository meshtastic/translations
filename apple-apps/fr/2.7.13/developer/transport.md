
# Couche de transport

`AccessoryManager`Abstraits BLE, TCP/IP et les transports en série derrière une seule interface. Les vues et les services interagissent uniquement avec`AccessoryManager`- Jamais avec des mises en œuvre de transport directement.

## Mise en œuvre du transport

Les transports vivent dans`Meshtastic/Accessory/Transports/`:

| Fichier | Protocole | Notes |
|------|----------|-------|
| `BLETransport.swift` | CoreBluetooth | Connexion BLE standard aux radios |
| `TCPTransport.swift` | Réseau.cadre | Wi-Fi / TCP/IP vers les radios avec réseau |
| `SerialTransport.swift` | Série IOKit | macOS uniquement ; adaptateurs USB-série |

Chaque transport est conforme à un`MeshTransport`Protocole qui expose`connect()`,`disconnect()`,`send(data:)`, Et un`received`Éditeur.

## Carte d'extension de AccessoryManager

| Extension | Méthodes clés |
|-----------|------------|
| `+Discovery` | `startScanning()`, `stopScanning()`, `peripheral(_:didDiscover:)` |
| `+Connect` | `connect(peripheral:)`, `disconnect()`, `centralManager(_:didConnect:)` |
| `+ToRadio` | `sendPacket(_:)`, `sendWantConfig()`, `sendWaypoint(_:)` |
| `+FromRadio` | `handleFromRadio(_:)`, `handleMeshPacket(_:)` |
| `+Position` | `startLocationUpdates()`, `sendPosition(_:)` |
| `+MQTT` | `connectMQTT()`, `publishPacket(_:)`, `mqttClient(_:didReceiveMessage:)` |
| `+TAK` | `handleATAKPluginPacket(_:)`,`handleATAKPluginV2Packet(_:)`,`handleATAKForwarderPacket(_:)`,`sendTAKPacket(_:channel:)`,`sendTAKV2Packet(_:channel:)`,`sendCoTToMeshV2(_:channel:)`. Voir [TAK Protocol](tak-protocol.html) pour le détail du format de fil V1/V2. |

## Flux de paquets (entrant)

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

## Flux de paquets (sortant)

```
View / Service
  → AccessoryManager+ToRadio.sendPacket(_:)
  → Encode to protobuf (ToRadio wrapper)
  → Transport.send(data:)
  → Radio
```

## Ajout d'un nouveau type de paquet

1. Ajoutez la définition de protobuf dans le`protobufs/`Sous-module.
2. Courir`./scripts/gen_protos.sh`.
3. Ajouter un dossier de décodage/expédition dans`AccessoryManager+FromRadio.handleFromRadio(_:)`.
4. Ajouter une méthode d'envoi dans`AccessoryManager+ToRadio.swift`.
5. Ajoutez une propriété de modèle ou une entité SwiftData si les données doivent persister.
6. Écrire des tests unitaires contre l'encodage/décodage aller-retour.

## Notes de concurrence

`AccessoryManager`N'est pas`@MainActor`. Son`@Published`Les propriétés sont observées à partir des vues SwiftUI sur l'acteur principal. Utiliser`await MainActor.run { }`Lors de la mise à jour des propriétés publiées à partir de tâches d'arrière-plan ou de rappels de délégués CoreBluetooth.

Les écritures de persistance d'arrière-plan doivent passer par le`MeshPackets` `@ModelActor`, Pas le principal`ModelContext`.

