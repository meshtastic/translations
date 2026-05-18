
# Guide de base de code

## Structure de haut niveau

```
Meshtastic/
├── MeshtasticApp.swift         # @main App struct
├── MeshtasticAppDelegate.swift # UIApplicationDelegate (SiriKit)
├── AppState.swift              # @EnvironmentObject root state
├── Accessory/                  # BLE/TCP/serial connectivity
├── API/                        # REST API helpers
├── AppIntents/                 # Siri / Shortcuts intents
├── CarPlay/                    # CarPlay scene
├── Enums/                      # Shared enumerations
├── Extensions/                 # Swift extensions (Logger, Date, String…)
├── Helpers/                    # Utility types (no UI)
├── Intents/                    # INIntent handlers
├── Measurement/                # Unit/measurement formatting
├── Model/                      # @Model SwiftData types
├── Persistence/                # PersistenceController, MeshPackets actor
├── Resources/                  # Assets, docs bundle, Info.plist
├── Router/                     # Router + NavigationState
├── Tips/                       # TipKit tips
└── Views/                      # SwiftUI views
    ├── Bluetooth/              # BLE connect view
    ├── Map/                    # Map + overlay views
    ├── Messages/               # Channel + DM views
    ├── Nodes/                  # Node list + detail
    └── Settings/               # All settings views
MeshtasticProtobufs/            # Swift Package wrapping generated protobufs
MeshtasticTests/                # Test target (Swift Testing)
scripts/                        # Build and utility scripts
specs/                          # Feature specs (speckit workflow)
```

## Fichiers clés

| Fichier | But |
|------|---------|
| `Router/Router.swift` | Contrôleur de navigation central (`@MainActor`) |
| `Router/NavigationState.swift` | Énumérations d'état de navigation par onglet |
| `Extensions/Logger.swift` | Enregistreurs OSLog tapés pour tous les sous-systèmes |
| `Persistence/PersistenceController.swift` | Données rapides`ModelContainer`Mise en place |
| `Model/MeshtasticSchema.swift` | `VersionedSchema` + `SchemaMigrationPlan` |
| `Accessory/Accessory Manager/AccessoryManager.swift` | Classe racine du gestionnaire BLE/TCP |

## Modèle de fichier d'extension

Les grandes classes de gestionnaires sont divisées en`+Extension`Fichiers regroupés par préoccupation :

```swift
// AccessoryManager.swift — properties and init only
// AccessoryManager+Connect.swift — connection lifecycle
// AccessoryManager+ToRadio.swift — outbound packet methods
// AccessoryManager+FromRadio.swift — inbound packet handling
```

Suivez le même schéma lors de l'ajout de nouveaux sous-systèmes à`AccessoryManager`Ou d'autres grandes classes.

## Abattage des arbres

Toutes les utilisations de journalisation typées`Logger`Instances de`Meshtastic/Extensions/Logger.swift`. Ne jamais utiliser`print()`.

```swift
Logger.mesh.debug("Packet received: \(packet.id)")
Logger.transport.error("BLE write failed: \(error)")
```

Catégories disponibles :`.admin`,`.data`,`.docs`,`.mesh`,`.mqtt`,`.radio`,`.services`,`.statistics`,`.transport`,`.tak`

## Voir la hiérarchie

Les vues sont en`Meshtastic/Views/`. Chaque fonctionnalité majeure a son propre sous-répertoire. La racine`ContentView`Hôtes un`TabView`Clé sur`NavigationState`.

Les vues qui ont besoin de connectivité injectent`@EnvironmentObject var bleManager: BLEManager`(Nom hérité ; code plus récent utilise`AccessoryManager`). Les vues qui ont besoin d'une navigation injectent`@EnvironmentObject var router: Router`.

## Localisation

Toutes les chaînes visibles par l'utilisateur doivent utiliser`String(localized:)`Ou`LocalizedStringKey`. Le fichier de chaînes source est`Localizable.xcstrings`Dans la racine du projet.

