
# Guía de base de código

## Estructura de nivel superior

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

## Archivos clave

| Ficha | Propósito |
|------|---------|
| `Router/Router.swift` | Controlador de navegación central (`@MainActor`) |
| `Router/NavigationState.swift` | Enumeraciones de estado de navegación por pestaña |
| `Extensions/Logger.swift` | Logadores OSLog mecanografiados para todos los subsistemas |
| `Persistence/PersistenceController.swift` | SwiftDatos`ModelContainer`Configuración |
| `Model/MeshtasticSchema.swift` | `VersionedSchema` + `SchemaMigrationPlan` |
| `Accessory/Accessory Manager/AccessoryManager.swift` | Clase raíz de administrador de BLE/TCP |

## Patrón de archivo de extensión

Las grandes clases de gerentes se dividen en`+Extension`Archivos agrupados por preocupación:

```swift
// AccessoryManager.swift — properties and init only
// AccessoryManager+Connect.swift — connection lifecycle
// AccessoryManager+ToRadio.swift — outbound packet methods
// AccessoryManager+FromRadio.swift — inbound packet handling
```

Siga el mismo patrón al agregar nuevos subsistemas a`AccessoryManager`U otras clases grandes.

## Tala de árboles

Todos los usos de registro mecanografiados`Logger`Instancias de`Meshtastic/Extensions/Logger.swift`. Nunca usar`print()`.

```swift
Logger.mesh.debug("Packet received: \(packet.id)")
Logger.transport.error("BLE write failed: \(error)")
```

Categorías disponibles:`.admin`,`.data`,`.docs`,`.mesh`,`.mqtt`,`.radio`,`.services`,`.statistics`,`.transport`,`.tak`

## Ver jerarquía

Las vistas están en`Meshtastic/Views/`. Cada característica principal tiene su propio subdirectorio. La raíz`ContentView`Organiza un`TabView`Encendido`NavigationState`.

Las vistas que necesitan conectividad inyectan`@EnvironmentObject var bleManager: BLEManager`(Nombre heredado; el código más nuevo utiliza`AccessoryManager`). Vistas que necesitan navegación inyecta`@EnvironmentObject var router: Router`.

## Localización

Todas las cadenas visibles para el usuario deben usar`String(localized:)`O`LocalizedStringKey`. El archivo de cadenas de origen es`Localizable.xcstrings`En la raíz del proyecto.

