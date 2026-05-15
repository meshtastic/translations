
# Codebase-Leitfaden
## Struktur auf oberster Ebene
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

## Schlüsseldateien
| Dossier | Zweck |
|------|---------|
| `Router/Router.swift` | Zentraler Navigationscontroller (`@MainActor`) |
| `Router/NavigationState.swift` | Per-Tab-Navigationsstatus-Enums |
| `Erweiterungen/Logger.swift` | Getippte OSLog-Logger für alle Subsysteme |
| `Persistence/PersistenceController.swift` | SwiftData `ModelContainer` Einrichtung |
| `Modell/MeshtasticSchema.swift` | `Versioniertes Schema` + `SchemaMigrationsplan` |
| `Zubehör/Zubehör-Manager/Zubehör-Manager.swift` | BLE/TCP-Manager-Stammklasse |

## Erweiterungsdateimuster
Große Managerklassen sind unterteilt in`+Extension`Dateien gruppiert nach Anliegen:
```swift
// AccessoryManager.swift — properties and init only
// AccessoryManager+Connect.swift — connection lifecycle
// AccessoryManager+ToRadio.swift — outbound packet methods
// AccessoryManager+FromRadio.swift — inbound packet handling
```

Befolgen Sie das gleiche Muster, wenn Sie neue Subsysteme zu`AccessoryManager`Oder andere große Klassen.
## Protokollierung
Alle Protokollierung verwendet typiert`Logger`Instanzen aus`Meshtastic/Extensions/Logger.swift`. Niemals verwenden`print()`.
```swift
Logger.mesh.debug("Packet received: \(packet.id)")
Logger.transport.error("BLE write failed: \(error)")
```

Verfügbare Kategorien:`.admin`,`.data`,`.docs`,`.mesh`,`.mqtt`,`.radio`,`.services`,`.statistics`,`.transport`,`.tak`

## Hierarchie anzeigen
Ansichten sind in`Meshtastic/Views/`. Jedes Hauptmerkmal hat sein eigenes Unterverzeichnis. Die Wurzel`ContentView`Gastgeber eines`TabView`Eingeschlüsselt`NavigationState`.
Ansichten, die Konnektivität benötigen, injizieren`@EnvironmentObject var bleManager: BLEManager`(Legacy-Name; neuerer Code verwendet`AccessoryManager`). Ansichten, die eine Navigation benötigen`@EnvironmentObject var router: Router`.
## Lokalisierung
Alle vom Benutzer sichtbaren Zeichenfolgen müssen`String(localized:)`Oder`LocalizedStringKey`. Die Quellzeichenfolgendatei ist`Localizable.xcstrings`Im Projektstamm.
