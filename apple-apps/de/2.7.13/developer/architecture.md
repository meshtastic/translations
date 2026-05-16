
# Architekturübersicht

Die Meshtastic Apple App richtet sich an iOS, iPadOS und macOS (über Mac Catalyst). Es kommuniziert mit Meshtastic-Funkgeräten über BLE, TCP/IP und (auf macOS) seriell.

## App-Einstiegspunkt

`Meshtastic/MeshtasticApp.swift`Ist der`@main` `App`Struktur. Beim Start:

1. Schafft`PersistenceController.shared`(SwiftData`ModelContainer`)
2. Instanzien`AppState`(Wraps`Router`)
3. Instanzien`AccessoryManager`(BLE/TCP/serielle Konnektivität)
4. Instanzien`AccessoryManager.shared`Als`@EnvironmentObject`Für die Ansichtshierarchie

`MeshtasticAppDelegate.swift`Griffe`UIApplicationDelegate`Haken für SiriKit CarPlay Messaging-Absichten.

## Router & Navigation

`Router`(`Meshtastic/Router/Router.swift`) Ist ein`@MainActor` `ObservableObject`Das besitzt ein`NavigationState`Struktur. Es treibt die Registerkartenauswahl und das Deep-Link-Routing voran.

```
Router
└── NavigationState
    ├── MessagesNavigationState   (tab 0)
    ├── MapNavigationState        (tab 1)
    ├── NodesNavigationState      (tab 2)
    └── SettingsNavigationState   (tab 3)
```

Deep Links verwenden die`meshtastic:///`URL-Schema.`Router.route(url:)`Parst den Pfad und setzt den entsprechenden Navigationszustand. Siehe [Deep Links](deep-links) für die vollständige URL-Referenz.

## AppStatus

`AppState`Wraps`Router`Und wird als`@EnvironmentObject`An der Wurzel der SwiftUI-Ansichtshierarchie. Ansichten, die programmatisch navigiert werden müssen`@EnvironmentObject var router: Router`Direkt - oder häufiger`@EnvironmentObject var appState: AppState`Und Zugang`appState.router`.

## Zubehör-Manager

`AccessoryManager`Ist der zentrale Konnektivitätsmanager, der auf Erweiterungsdateien aufgeteilt ist:

| Dossier | Verantwortlichkeit |
|------|---------------|
| `AccessoryManager+Discovery.swift` | BLE-Scannen, Geräteerkennung |
| `AccessoryManager+Connect.swift` | Verbindungslebenszyklus, Logik der Wiederverbindung |
| `AccessoryManager+ToRadio.swift` | Pakete an das Radio gesendet |
| `AccessoryManager+FromRadio.swift` | Vom Radio empfangene Pakete |
| `AccessoryManager+Position.swift` | GPS-Positionsfreigabe |
| `AccessoryManager+MQTT.swift` | MQTT-Proxy |
| `AccessoryManager+TAK.swift` | TAK/CoT-Integration |

Transportprotokolle sind in`Meshtastic/Accessory/Transports/`.

## Fortdauer

SwiftData ist die einzige Persistenzschicht.`PersistenceController.shared`Besitzt die`ModelContainer`. Ansichten verwenden`@Environment(\.modelContext)`Oder`@Query`. Hintergrund schreibt, verwenden Sie die`MeshPackets` `@ModelActor`.

Modelltypen werden definiert mit`@Model`In`Meshtastic/Model/`. Schema-Evolution verwendet`VersionedSchema`Und`SchemaMigrationPlan`In`MeshtasticSchema.swift`.

## Dienstleistung

Anwendungsdienste, die nicht an die Funkverbindung gebunden sind, leben in`Meshtastic/Services/`.

| Dossier | Verantwortlichkeit |
|------|---------------|
| `DocTranslationService.swift` | Dokumentationsübersetzung auf dem Gerät mit dem Apple Translation Framework (primär) mit FoundationModels Fallback. Erkennt die Sprache des Geräts, übersetzt gebündelte englische Docs-Seiten und verwaltet die Hintergrund-Voreinstellung. iOS 26+. |
| `TranslationCache.swift` | Dateibasierter Cache für übersetzte`.md`Inhalt, der im Anwendungssupport gespeichert ist. Verfolgt Inhalts-Hashes zur Erkennung von Staless und setzt eine 50 MB pro Sprache LRU-Räufungsrichtlinie durch. |

## Protobufs

Die`MeshtasticProtobufs`Swift-Paket (`MeshtasticProtobufs/Package.swift`) Umschließt Protobuf-generierte Swift-Quellen. Regenerieren mit`./scripts/gen_protos.sh`Nach dem Aktualisieren der`protobufs/`Untermodul.

## Externe Swift-Pakete

| Bündel | Zweck |
|---------|---------|
| [TAKPacket-SDK](https://github.com/meshtastic/TAKPacket-SDK) | TAK V2 Drahtformat auf`ATAK_PLUGIN_V2 = port 78`. Entlarvt`CotXmlParser`,`CotXmlBuilder`, Und`TakCompressor`(Zstd Wörterbuchkomprimierung). Angeheftet`Meshtastic.xcworkspace/.../Package.resolved`. Siehe [TAK-Protokoll](tak-protocol.html). |

