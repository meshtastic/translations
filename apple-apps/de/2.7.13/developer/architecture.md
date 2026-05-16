
# Architekturübersicht

Die Meshtastic Apple App richtet sich an iOS, iPadOS, macOS (über Mac Catalyst), watchOS und visionOS. Es kommuniziert mit Meshtastic-Funkgeräten über BLE, TCP/IP und (auf macOS) seriell.

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
| `Zubehör-Manager+Discovery.swift` | BLE-Scannen, Geräteerkennung |
| `Zubehör-Manager+Verbinden.swift` | Verbindungslebenszyklus, Logik der Wiederverbindung |
| `Zubehör-Manager+ToRadio.swift` | Pakete an das Radio gesendet |
| `Zubehör-Manager+VonRadio.swift` | Vom Radio empfangene Pakete |
| `Zubehör-Manager+Position.swift` | GPS-Positionsfreigabe |
| `Zubehör-Manager+MQTT.swift` | MQTT-Proxy |
| `Zubehör-Manager+TAK.swift` | TAK/CoT-Integration |

Transportprotokolle sind in`Meshtastic/Accessory/Transports/`.

## Fortdauer

SwiftData ist die einzige Persistenzschicht.`PersistenceController.shared`Besitzt die`ModelContainer`. Ansichten verwenden`@Environment(\.modelContext)`Oder`@Query`. Hintergrund schreibt, verwende die`MeshPackets` `@ModelActor`.

Modelltypen werden definiert mit`@Model`In`Meshtastic/Model/`. Schema-Evolution verwendet`VersionedSchema`Und`SchemaMigrationPlan`In`MeshtasticSchema.swift`.

## Dienstleistung

Anwendungsdienste, die nicht an die Funkverbindung gebunden sind, leben in`Meshtastic/Services/`.

| Dossier | Verantwortlichkeit |
|------|---------------|
| `DocTranslationService.swift` | Dokumentationsübersetzung auf dem Gerät mit dem Apple Translation Framework (primär) mit FoundationModels Fallback. Übersetzt gebündelte englische Markdown-Quelldateien, zwischenspeichert übersetzte `.md`, konvertiert über `MarkdownConverter` in HTML und löst den automatischen Upload nach dem Prepetch aus. iOS 26+. |
| `TranslationCache.swift` | Dateibasierter Cache für übersetzte `.md`-Inhalte, die in der Anwendungsunterstützung gespeichert sind. Verfolgt Inhalts-Hashes zur Erkennung von Staless und setzt eine 50 MB pro Sprache LRU-Räufungsrichtlinie durch. |
| `MarkdownConverter.swift` | GFM-kompatibles Markdown→HTML-Konverter. Unterstützt Überschriften, Absätze, Listen, Code-Zäune, Inline-Code, Tabellen, Links, Bilder, HTML-Passthrough (`<picture>`, `<img>`), Blockquote-Callouts (Tipp/Warnung), fett, kursiv, durchgestrichen, horizontale Regeln und `.md` → `.html` Link-Rewrite. Streifen YAML-Front-Matter und Jekyll-Inline-Attribute. |
| `DokumenteÜbersetzungUploader.swift` | Committ automatisch übersetzte `.md`-Dateien in das `meshtastic/translations`-Repo, nachdem der Hintergrund-Prefetch abgeschlossen ist. Führt schreibgeschützte Prüfungen gegen `meshtastic/meshtastic` und `meshtastic/translations` (keine Auth) durch und committ dann über die GitHub Contents API mit einem feinkörnigen PAT von `Secrets.json`. Die Verfolgung pro Datei ermöglicht einen erneuten Versuch von fehlgeschlagenen Uploads. |

## Protobufs

Die`MeshtasticProtobufs`Swift-Paket (`MeshtasticProtobufs/Package.swift`) Umschließt Protobuf-generierte Swift-Quellen. Regenerieren mit`./scripts/gen_protos.sh`Nach dem Aktualisieren der`protobufs/`Untermodul.

