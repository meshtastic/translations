
# Schnelle Daten

Die App verwendet SwiftData ausschließlich zur Persistenz. Stellen du SQLite, Realm, Core Data oder eine andere Persistenzbibliothek nicht vor.

## ModelContainer-Einrichtung

`PersistenceController.shared`(In`Meshtastic/Persistence/PersistenceController.swift`) Erstellt und besitzt die`ModelContainer`. Es wird einmal beim App-Start in initialisiert`MeshtasticApp.swift`Und injiziert über`.modelContainer(PersistenceController.shared.container)`.

Autosave ist in der Produktion **deaktiviert** (`container.mainContext.autosaveEnabled = false`). Alle Beharrlichkeit wird von expliziten`modelContext.save()`Ruft auf, damit die App genau kontrolliert, wann SQLite-Schreibvorgänge stattfinden.

## Strategie speichern

Die App verwendet je nach Pakethäufigkeit zwei Speichermuster:

### Sofortige Speicherungen

Konfiguration von Änderungen, Nachrichten, Wegpunkten und anderen niederfrequenten Mutationsaufrufen`savePendingChanges()`Direkt nach der Aktualisierung des Modelldiagramms. Dieser Helfer ist eine dünne Verpackung um`modelContext.save()`Mit Fehlerprotokollierung.

### Entbeutete Einsparungen

Hochfrequenz-Pakete - Positionen und Telemetrie - Verwendung`scheduleDebouncedSave()`Zu konalesce schreibt. Der Debouncer wartet **2 Sekunden** der Inaktivität, bevor er spült, mit einer harten Obergrenze von **5 Sekunden** ab dem ersten schmutzigen Wechsel. Dies verhindert Dutzende von SQLite-Schreibvorgängen pro Sekunde, wenn das Mesh besetzt ist.

```
Position packet 1 → dirty flag set, 2s timer starts
Position packet 2 (200ms later) → timer resets to 2s
Position packet 3 (200ms later) → timer resets to 2s
...
No packets for 2s → save() fires
— OR —
5s since first dirty change → save() fires regardless
```

Debounced-Speicher werden bei der Trennung explizit gespült, damit keine Daten verloren gehen.

## Indizes

Häufig abgefragte Felder verwenden`@Attribute(.unique)`Um einen EINZIGARTIGEN INDEX im zugrunde liegenden SQLite-Store zu erstellen. Dadurch werden vollständige Tabellenscans auf den heißesten Nachschlagpfaden eliminiert:

| Feld | Entität | Warum |
|-------|--------|-----|
| `num` | `KnotenInfoEntität` | Jedes eingehende Paket wurde nachgeschlagen |
| `num` | `Benutzerentität` | Ich habe jede Nachricht nachgeschlagen |
| `Nachrichten-ID` | `Nachrichtenentität` | ACK-Lookups, Deduplikation |
| `hwModell` | `Gerätehardwareeinheit` | Hardware-Image-Lookups |

> **Hinweis** —`@Attribute(.indexed)`Erfordert iOS 18+. Die App zielt auf iOS 17.5 ab, also`@Attribute(.unique)`Wird stattdessen verwendet (es erstellt einen EINZIGARTIGEN INDEX, der auch als regulärer Index dient).

## Verwendung des ModelContext in Ansichten

```swift
struct MyView: View {
    @Environment(\.modelContext) private var context
    @Query private var nodes: [NodeInfoEntity]

    var body: some View { ... }
}
```

Verwenden`@Query`Für Daten, die die Ansicht antreiben. Nutzung`context.insert(_:)`/`context.delete(_:)`Für Mutationen. Mutationen im Hauptkontext sind für den Hauptakteur sicher.

## Hintergrund schreibt

Für Schreibvorgänge, die von eingehenden Funkpaketen (aus dem Hauptthread) ausgelöst werden, verwenden du die`MeshPackets` `@ModelActor`:

```swift
let actor = MeshPackets(modelContainer: PersistenceController.shared.container)
await actor.savePacket(packet)
```

Schreiben du niemals an die wichtigsten`ModelContext`Aus einem Hintergrundthread.

## Modelltypen

Alle Modelltypen leben in`Meshtastic/Model/`. Jeder Typ ist verziert mit`@Model`:

```swift
@Model
final class NodeInfoEntity {
    var num: Int64
    var longName: String?
    // ...
}
```

Schlüsselmodelltypen:

| Art | Beschreibung |
|------|-------------|
| `KnotenInfoEntität` | Ein Knoten, der auf dem Netz gehört wird |
| `Nachrichtenentität` | Ein Kanal oder eine Direktnachricht |
| `PositionEntität` | Eine GPS-Positionsaktualisierung |
| `TelemetrieEntität` | Geräte-/Umgebungssensordaten |
| `TraceRouteEntity` | Eine aufgezeichnete Spurroute |
| `WegpunktEntität` | Ein gemeinsamer Kartenwegpunkt |

## Schema-Migrationen

Wenn du Eigenschaften auf einem`@Model`Typ, müssen du eine Migration bereitstellen. Schemadateien leben in`Meshtastic/Model/Schema/`.

### Hinzufügen einer neuen Schema-Version

1. Schaffen`Meshtastic/Model/Schema/MeshtasticSchemaV2.swift`Mit den aktualisierten Modellen:

```swift
enum MeshtasticSchemaV2: VersionedSchema {
    static var versionIdentifier = Schema.Version(2, 0, 0)
    static var models: [any PersistentModel.Type] { ... }
}
```

2. Anhinden`MeshtasticSchemaV2.self`Zu`MeshtasticMigrationPlan.schemas`(Neuest zuletzt).
3. Fügen du eine Migrationsstufe hinzu`MeshtasticMigrationPlan.stages`:

```swift
// Lightweight — SwiftData infers additive changes automatically (new optional properties)
static let migrateV1toV2 = MigrationStage.lightweight(
    fromVersion: MeshtasticSchemaV1.self,
    toVersion: MeshtasticSchemaV2.self
)

// Custom — when you need to transform or backfill data
static let migrateV1toV2 = MigrationStage.custom(
    fromVersion: MeshtasticSchemaV1.self,
    toVersion: MeshtasticSchemaV2.self,
    willMigrate: { context in },
    didMigrate: { context in
        // Transform data, populate new fields, etc.
        try context.save()
    }
)
```

4. Auf den neuesten Stand bringen`MeshtasticSchema.current`Auf die neue Version hinweisen.

> **Warning — Löschen du niemals eine`VersionedSchema`. ** Der Migrationsverlauf muss beibehalten werden, da der Migrationsplan auf Geräten, die Zwischenversionen übersprungen haben, fehlschlägt.

## Abfragehelfer

`QuerySwiftData.swift`Enthält Hilfsfunktionen für gängige Abfrechen:

```swift
let node = getNodeInfo(id: nodeNum, context: context)
```

`UpdateSwiftData.swift`Enthält Helfer für Upsert-Muster:

```swift
upsertNode(packet: packet, context: context)
```

Bevorzugen du diese Helfer gegenüber direkten Abfragen, um die Logik konsistent zu halten.

## Datenobergrenzen

Um das unbegrenzente Datenbankwachstum zu verhindern, erzwingt die App Obergrenzen pro Knoten beim Einfügen neuer Datensätze. Ältere Zeilen jenseits der Obergrenze werden in derselben Transaktion gelöscht:

| Geschäftsbeziehung | Kappe | Verhalten |
|-------------|-----|-----------|
| `KnotenInfoEntität.Positionen` | 5 000 | Älteste Positionen bei Überschreitung gelöscht |
| `NodeInfoEntity.Telemetrie` | 5 000 pro Metriktyp | Älteste Telemetrie dieses Typs gelöscht |
| `Nachrichtenentität` (pro Kanal) | 50 000 | Älteste Nachrichten im Kanal gelöscht |

Diese Obergrenzen werden in`UpdateSwiftData.swift`Während des Upsert-Pfads, so dass sie auf jedem eingehenden Paket ausgeführt werden, ohne dass eine separate Wartungsaufgabe erforderlich ist.

