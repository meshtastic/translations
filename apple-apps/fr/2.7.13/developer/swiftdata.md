
# Données rapides

L'application utilise SwiftData exclusivement pour la persistance. N'introduisez pas SQLite, Realm, Core Data ou toute autre bibliothèque de persistance.

## Configuration du conteneur de modèle

`PersistenceController.shared`(En`Meshtastic/Persistence/PersistenceController.swift`) Crée et possède le`ModelContainer`. Il est initialisé une fois au lancement de l'application dans`MeshtasticApp.swift`Et injecté via`.modelContainer(PersistenceController.shared.container)`.

L'enregistrement automatique est **désabilié** en production (`container.mainContext.autosaveEnabled = false`). Toute persistance est motivée par l'explicite`modelContext.save()`Appelle pour que l'application contrôle exactement quand les écritures SQLite se produisent.

## Stratégie de sauvegarde

L'application utilise deux modèles de sauvegarde en fonction de la fréquence des paquets :

### Sauvegardes immédiates

Modifications de configuration, messages, waypoints et autres appels de mutations à basse fréquence`savePendingChanges()`Directement après la mise à jour du graphique du modèle. Cette aide est une fine enveloppe autour de`modelContext.save()`Avec erreur de journalisation.

### Sauvetes débounqués

Paquets à haute fréquence - positions et télémétrie - utilisation`scheduleDebouncedSave()`À coalesce écrit. Le débouncer attend **2 secondes** d'inactivité avant de rincer, avec un plafond dur de **5 secondes** à partir du premier changement sale. Cela empêche des dizaines d'écritures SQLite par seconde lorsque le maillage est occupé.

```
Position packet 1 → dirty flag set, 2s timer starts
Position packet 2 (200ms later) → timer resets to 2s
Position packet 3 (200ms later) → timer resets to 2s
...
No packets for 2s → save() fires
— OR —
5s since first dirty change → save() fires regardless
```

Les sauvegardes débounées sont vidées explicitement lors de la déconnexion afin qu'aucune donnée ne soit perdue.

## Indices

Les champs fréquemment interrogés utilisent`@Attribute(.unique)`Pour créer un INDEX UNIQUE dans le magasin SQLite sous-jacent. Cela élimine les analyses de tableau complètes sur les chemins de recherche les plus chauds :

| Champ | Entité | Pourquoi |
|-------|--------|-----|
| `num` | `NodeInfoEntity` | J'ai regardé chaque paquet entrant |
| `num` | `UserEntity` | J'ai regardé chaque message |
| `messageId` | `MessageEntity` | ACK recherches, déduplication |
| `hwModel` | `DeviceHardwareEntity` | Recherches d'images matérielles |

> **Remarque** —`@Attribute(.indexed)`Nécessite iOS 18+. L'application cible iOS 17.5, donc`@Attribute(.unique)`Est utilisé à la place (il crée un INDEX UNIQUE qui sert également d'index régulier).

## Utilisation du ModelContext dans les vues

```swift
struct MyView: View {
    @Environment(\.modelContext) private var context
    @Query private var nodes: [NodeInfoEntity]

    var body: some View { ... }
}
```

Utiliser`@Query`Pour les données qui alimentent la vue. Utiliser`context.insert(_:)`/`context.delete(_:)`Pour les mutations. Les mutations sur le contexte principal sont sûres sur l'acteur principal.

## Écrit en arrière-plan

Pour les écritures déclenchées par les paquets radio entrants (hors du thread principal), utilisez le`MeshPackets` `@ModelActor`:

```swift
let actor = MeshPackets(modelContainer: PersistenceController.shared.container)
await actor.savePacket(packet)
```

N'écrivez jamais au principal`ModelContext`À partir d'un fil de discussion d'arrière-plan.

## Types de modèles

Tous les types de modèles vivent dans`Meshtastic/Model/`. Chaque type est décoré avec`@Model`:

```swift
@Model
final class NodeInfoEntity {
    var num: Int64
    var longName: String?
    // ...
}
```

Types de modèles clés :

| Type | Description |
|------|-------------|
| `NodeInfoEntity` | Un nœud entendu sur le maillage |
| `MessageEntity` | Un canal ou un message direct |
| `PositionEntity` | Une mise à jour de la position GPS |
| `TelemetryEntity` | Données du capteur de l'appareil/de l'environnement |
| `TraceRouteEntity` | Un itinéraire de trace enregistré |
| `WaypointEntity` | Un waypoint de carte partagé |

## Migrations de schéma

Lorsque vous ajoutez, renommez ou supprimez des propriétés sur un`@Model`Type, vous devez fournir une migration. Les fichiers de schéma en direct dans`Meshtastic/Model/Schema/`.

### Ajout d'une nouvelle version du schéma

1. Créer`Meshtastic/Model/Schema/MeshtasticSchemaV2.swift`Avec les modèles mis à jour :

```swift
enum MeshtasticSchemaV2: VersionedSchema {
    static var versionIdentifier = Schema.Version(2, 0, 0)
    static var models: [any PersistentModel.Type] { ... }
}
```

2. Ajouter`MeshtasticSchemaV2.self`À`MeshtasticMigrationPlan.schemas`(Le plus récent dernier).
3. Ajouter une étape de migration à`MeshtasticMigrationPlan.stages`:

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

4. Mise à jour`MeshtasticSchema.current`Pour pointer vers la nouvelle version.

> **Warning — Ne supprimez jamais un`VersionedSchema`. ** L'historique des migrations doit être préservé ou le plan de migration échouera sur les appareils qui ont ignoré les versions intermédiaires.

## Aides aux requêtes

`QuerySwiftData.swift`Contient des fonctions d'aide pour les récupérations courantes :

```swift
let node = getNodeInfo(id: nodeNum, context: context)
```

`UpdateSwiftData.swift`Contient des aides pour les modèles de mise en valeur :

```swift
upsertNode(packet: packet, context: context)
```

Préférez ces assistants aux requêtes directes pour garder la logique cohérente.

## Plafonds de données

Pour empêcher la croissance illimitée de la base de données, l'application applique des plafonds par nœud lors de l'insertion de nouveaux enregistrements. Les lignes plus anciennes au-delà du plafond sont supprimées dans la même transaction :

| Relation | Casquette | Comportement |
|-------------|-----|-----------|
| `NodeInfoEntity.positions` | 5 000 | Les positions les plus anciennes sont supprimées lorsqu'elles sont dépassées |
| `NodeInfoEntity.telemetries` | 5 000 par type de métriques | La plus ancienne télémétrie de ce type a été supprimée |
| `MessageEntity`(Par canal) | 50 000 | Les messages les plus anciens de la chaîne supprimés |

Ces bouchons sont appliqués en`UpdateSwiftData.swift`Pendant le chemin de mise à niveau, ils s'exécutent donc sur chaque paquet entrant sans nécessiter de tâche de maintenance distincte.

