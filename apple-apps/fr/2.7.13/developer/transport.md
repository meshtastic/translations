
# Couche de transport
`AccessoryManager` fait abstraction des transports BLE, TCP/IP et série derrière une seule interface. Les vues et les services interagissent uniquement avec `AccessoryManager` - jamais avec les implémentations de transport directement.
## Mise en œuvre du transport
Les transports vivent dans `Meshtastic/Accessory/Transports/` :
| Fichier | Protocole | Notes |
|------|----------|-------|
| `BLETransport.swift` | CoreBluetooth | Connexion BLE standard aux radios |
| `TCPTransport.swift` | Réseau.cadre | Wi-Fi / TCP/IP vers les radios avec réseau |
| `Transport en série.swift` | Série IOKit | macOS uniquement ; adaptateurs USB-série |

Chaque transport est conforme à un protocole `MeshTransport` qui expose `connect()`, `disconnect()`, `send(data:)` et un éditeur `received`.
## Carte d'extension de AccessoryManager
| Extension | Méthodes clés |
|-----------|------------|
| `+Découverte` | `startScanning()`, `stopScanning()`, `peripheral(_:didDiscover:)` |
| `+Connecter` | `connect(peripheral:)`, `disconnect()`, `centralManager(_:didConnect:)` |
| `+ToRadio` | `sendPacket(_:)`, `sendWantConfig()`, `sendWaypoint(_:)` |
| `+De la radio` | `handleFromRadio(_:)`, `handleMeshPacket(_:)` |
| `+Position` | `startLocationUpdates()`, `sendPosition(_:)` |
| `+MQTT` | `connectMQTT()`, `publishPacket(_:)`, `mqttClient(_:didReceiveMessage:)` |
| `+TAK` | `handleATAKPluginPacket(_:)`, `handleATAKPluginV2Packet(_:)`, `handleATAKForwarderPacket(_:)`, `sendTAKPacket(_:channel:)`, `sendTAKV2Packet(_:channel:)`, `sendCoTToMeshV2(_:channel:)`. Voir [TAK Protocol](tak-protocol.html) pour le détail du format de fil V1/V2. |

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
1. Ajoutez la définition de protobuf dans le sous-module `protobufs/`.2. Exécutez `./scripts/gen_protos.sh`.3. Ajoutez un dossier de décodage/dispatch dans `AccessoryManager+FromRadio.handleFromRadio(_:)`.4. Ajoutez une méthode d'envoi dans `AccessoryManager+ToRadio.swift`.5. Ajoutez une propriété de modèle ou une entité SwiftData si les données doivent persister.6. Écrire des tests unitaires contre l'encodage/décodage aller-retour.
## Notes de concurrence
`AccessoryManager` n'est pas `@MainActor`. Ses propriétés `@Published` sont observées à partir des vues SwiftUI sur l'acteur principal. Utilisez `await MainActor.run { }` lors de la mise à jour des propriétés publiées à partir de tâches d'arrière-plan ou de rappels de délégués CoreBluetooth.
Les écritures de persistance d'arrière-plan doivent passer par les `MeshPackets` `@ModelActor`, et non par le `ModelContext` principal.
