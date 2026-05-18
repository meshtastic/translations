
# SwiftDatos

La aplicación utiliza SwiftData exclusivamente para la persistencia. No introduzca SQLite, Realm, Core Data ni ninguna otra biblioteca de persistencia.

## Configuración de ModelContainer

`PersistenceController.shared`(En`Meshtastic/Persistence/PersistenceController.swift`) Crea y posee el`ModelContainer`. Se inicializa una vez en el lanzamiento de la aplicación en`MeshtasticApp.swift`E inyectado a través de`.modelContainer(PersistenceController.shared.container)`.

El guardado automático está **deshabilitado** en producción (`container.mainContext.autosaveEnabled = false`). Toda persistencia es impulsada por explícito`modelContext.save()`Llamadas para que la aplicación controle exactamente cuándo se producen las escrituras de SQLite.

## Guardar estrategia

La aplicación utiliza dos patrones de guardado dependiendo de la frecuencia del paquete:

### Guardados inmediatos

Cambios de configuración, mensajes, waypoints y otras llamadas de mutaciones de baja frecuencia`savePendingChanges()`Directamente después de actualizar el gráfico del modelo. Este ayudante es una envoltura delgada alrededor`modelContext.save()`Con error de registro.

### Salvaciones desbounced

Paquetes de alta frecuencia, posiciones y telemetría, uso`scheduleDebouncedSave()`A coalesce escribe. El debouncer espera **2 segundos** de inactividad antes de enjuagarse, con un techo duro de **5 segundos** desde el primer cambio sucio. Esto evita docenas de escrituras de SQLite por segundo cuando la malla está ocupada.

```
Position packet 1 → dirty flag set, 2s timer starts
Position packet 2 (200ms later) → timer resets to 2s
Position packet 3 (200ms later) → timer resets to 2s
...
No packets for 2s → save() fires
— OR —
5s since first dirty change → save() fires regardless
```

Los guardados debounced se eliminan explícitamente al desconectarse para que no se pierdan datos.

## Índices

Los campos consultados con frecuencia utilizan`@Attribute(.unique)`Para crear un ÍNDICE ÚNICO en la tienda SQLite subyacente. Esto elimina los escaneos completos de la tabla en las rutas de búsqueda más populares:

| Campo | Entidad | Por qué |
|-------|--------|-----|
| `num` | `NodeInfoEntity` | Busqué en cada paquete entrante |
| `num` | `UserEntity` | Busqué cada mensaje |
| `messageId` | `MessageEntity` | ACK búsquedas, deduplicación |
| `hwModel` | `DeviceHardwareEntity` | Buscaciones de imágenes de hardware |

> **Nota** —`@Attribute(.indexed)`Requiere iOS 18+. La aplicación está dirigida a iOS 17.5, así que`@Attribute(.unique)`Se utiliza en su lugar (crea un ÍNDICE ÚNICO que también sirve como índice regular).

## Usando el ModelContext en Vistas

```swift
struct MyView: View {
    @Environment(\.modelContext) private var context
    @Query private var nodes: [NodeInfoEntity]

    var body: some View { ... }
}
```

Uso`@Query`Para los datos que impulsan la vista. Uso`context.insert(_:)`/`context.delete(_:)`Para mutaciones. Las mutaciones en el contexto principal son seguras en el actor principal.

## Escritos de fondo

Para las escrituras activadas por paquetes de radio entrantes (fuera del hilo principal), use el`MeshPackets` `@ModelActor`:

```swift
let actor = MeshPackets(modelContainer: PersistenceController.shared.container)
await actor.savePacket(packet)
```

Nunca escribas al principal`ModelContext`De un hilo de fondo.

## Tipos de modelos

Todos los tipos de modelos viven en`Meshtastic/Model/`. Cada tipo está decorado con`@Model`:

```swift
@Model
final class NodeInfoEntity {
    var num: Int64
    var longName: String?
    // ...
}
```

Tipos de modelos clave:

| Tipo | Descripción |
|------|-------------|
| `NodeInfoEntity` | Un nodo escuchado en la malla |
| `MessageEntity` | Un canal o mensaje directo |
| `PositionEntity` | Una actualización de la posición del GPS |
| `TelemetryEntity` | Datos del sensor del dispositivo/ambiente |
| `TraceRouteEntity` | Una ruta de seguimiento registrada |
| `WaypointEntity` | Un waypoint del mapa compartido |

## Migraciones de esquema

Cuando añades, renombras o eliminas propiedades en un`@Model`Tipo, debe proporcionar una migración. Los archivos de esquema viven en`Meshtastic/Model/Schema/`.

### Agregar una nueva versión del esquema

1. Crear`Meshtastic/Model/Schema/MeshtasticSchemaV2.swift`Con los modelos actualizados:

```swift
enum MeshtasticSchemaV2: VersionedSchema {
    static var versionIdentifier = Schema.Version(2, 0, 0)
    static var models: [any PersistentModel.Type] { ... }
}
```

2. Añadir`MeshtasticSchemaV2.self`A`MeshtasticMigrationPlan.schemas`(Último más nuevo).
3. Añade una etapa de migración a`MeshtasticMigrationPlan.stages`:

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

4. Actualización`MeshtasticSchema.current`Para señalar la nueva versión.

> **Warning — Nunca borres un`VersionedSchema`. ** El historial de migración debe conservarse o el plan de migración fallará en los dispositivos que se hayan omitido las versiones intermedias.

## Ayudantes de consulta

`QuerySwiftData.swift`Contiene funciones auxiliares para búsquedas comunes:

```swift
let node = getNodeInfo(id: nodeNum, context: context)
```

`UpdateSwiftData.swift`Contiene ayudantes para patrones de actualización:

```swift
upsertNode(packet: packet, context: context)
```

Prefiere estos ayudantes a las consultas directas para mantener la lógica consistente.

## Límites de datos

Para evitar el crecimiento sin límites de la base de datos, la aplicación aplica límites por nodo al insertar nuevos registros. Las filas más antiguas más allá del límite se eliminan en la misma transacción:

| Relación | Gorra | Conducta |
|-------------|-----|-----------|
| `NodeInfoEntity.positions` | 5 000 | Las posiciones más antiguas eliminadas cuando se superan |
| `NodeInfoEntity.telemetries` | 5 000 por tipo de métricas | La telemetría más antigua de ese tipo eliminada |
| `MessageEntity`(Por canal) | 50 000 | Mensajes más antiguos del canal eliminados |

Estas tapas se aplican en`UpdateSwiftData.swift`Durante la ruta ascendente, por lo que se ejecutan en cada paquete entrante sin requerir una tarea de mantenimiento separada.

