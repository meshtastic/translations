
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

## Pruebas de rendimiento - Arnés de semillas de base de datos grande

`Meshtastic/Persistence/PerformanceSeedData.swift`Proporciona un arnés solo para DEBUG para sembrar miles de nodos sintéticos, filas de telemetría, posiciones y mensajes en el almacén del simulador. Está completamente cerrado por banderas de tiempo de lanzamiento; las compilaciones de producción y las compilaciones DEBUG no lanzadas no se ven afectadas.

### Activando el arnés

El arnés se activa cuando **cualquiera de los siguientes está presente en el lanzamiento:

- El`--meshtastic-perf-seed`Argumento de lanzamiento, **o**
- El`MESHTASTIC_PERF_SEED_NODES`Variable de entorno (cualquier valor entero distinto de cero)

Cuando ninguno está establecido,`PerformanceSeedData.configuration`Devoluciones`nil`Y no se ejecuta ningún código semilla.

### Variables del entorno

Pasa las variables al simulador usando el`SIMCTL_CHILD_`Prefijo (el prefijo se elimina antes de que la aplicación los vea):

| Variable | Valor por defecto | Descripción |
|----------|---------|-------------|
| `MESHTASTIC_PERF_SEED_NODES` | — | **Requerido para activar. ** Número de nodos para sembrar (p. ej.`5000`). |
| `MESHTASTIC_PERF_TELEMETRY_HISTORY` | `3` | Muestras métricas de dispositivo + entorno por nodo. |
| `MESHTASTIC_PERF_LOCAL_STATS_HISTORY` | `MESHTASTIC_PERF_TELEMETRY_HISTORY` | Muestras de estadísticas locales por nodo, incluyendo piso de ruido sintético, contadores de paquetes, utilización y recuentos de nodos. |
| `MESHTASTIC_PERF_POSITION_HISTORY` | `3` | Entradas del historial de posiciones por nodo. |
| `MESHTASTIC_PERF_DIRECT_MESSAGES` | `0` | Mensajes directos a la semilla entre el nodo 0 y el nodo 1. |
| `MESHTASTIC_PERF_CHANNEL_MESSAGES` | `0` | Canale los mensajes a la semilla en el canal 0. |
| `MESHTASTIC_PERF_RESET_STORE` | `0` | Establecer en`1`/`true`Para despejar la tienda antes de sembrar. |
| `MESHTASTIC_PERF_COMPACT_LIST` | `0` | Establecer en`1`/`true`Para cambiar la lista de nodos a densidad compacta. |
| `MESHTASTIC_PERF_ENABLE_DISCOVERY` | `0` | Establecer en`1`/`true`Para dejar habilitado el descubrimiento de BLE (desactivado por defecto para las ejecuciones de rendimiento). |

### Ejemplo: sembrar 5 000 nodos con una tienda limpia

Primero, encuentre su simulador UDID:

```bash
xcrun simctl list devices booted
```

A continuación, inicie con las variables semilla:

```bash
SIMCTL_CHILD_MESHTASTIC_PERF_SEED_NODES=5000 \
SIMCTL_CHILD_MESHTASTIC_PERF_RESET_STORE=true \
SIMCTL_CHILD_MESHTASTIC_PERF_COMPACT_LIST=true \
xcrun simctl launch <UDID> gvh.MeshtasticClient
```

### Ejemplo: estadísticas locales de semillas para el trabajo de gráficos de suelo de ruido

Utilice un recuento de nodos más pequeño y un historial de estadísticas locales más grande al ajustar la interfaz de usuario del registro de estadísticas locales. Esto mantiene el simulador receptivo al tiempo que le da al gráfico suficiente variación para mostrar períodos de silencio, períodos ocupados y picos de interferencia ocasionales.

```bash
SIMCTL_CHILD_MESHTASTIC_PERF_SEED_NODES=20 \
SIMCTL_CHILD_MESHTASTIC_PERF_LOCAL_STATS_HISTORY=168 \
SIMCTL_CHILD_MESHTASTIC_PERF_TELEMETRY_HISTORY=3 \
SIMCTL_CHILD_MESHTASTIC_PERF_POSITION_HISTORY=3 \
SIMCTL_CHILD_MESHTASTIC_PERF_RESET_STORE=true \
SIMCTL_CHILD_MESHTASTIC_PERF_ENABLE_DISCOVERY=0 \
xcrun simctl launch <UDID> gvh.MeshtasticClient \
  --meshtastic-perf-seed \
  --meshtastic-perf-start-local-stats
```

`--meshtastic-perf-start-local-stats`Selecciona el nodo sembrado`0x0A000000`Y abre su registro de estadísticas locales directamente en las compilaciones del simulador DEBUG.

Añadir`--meshtastic-perf-local-stats-same-hour`Al comprobar el diseño del gráfico del piso de ruido de corto alcance. Mantiene muestras de estadísticas locales en la misma hora a intervalos de 5 minutos, lo que hace que`1h`Recorte de etiquetas de eje fácil de reproducir.

En lanzamientos posteriores **sin**`MESHTASTIC_PERF_RESET_STORE`, El arnés detecta el recuento de nodos existente y se salta la resiembra, por lo que la aplicación se inicia a toda velocidad contra la tienda ya sembrada.

### Qué esperar

5 000 nodos (3 muestras de telemetría de dispositivos/entorno, 3 muestras de estadísticas locales, 3 posiciones/nodo) semillas en aproximadamente **12 segundos** en un Apple Silicon Mac. La aplicación navega automáticamente a la pestaña Nodos. La CPU inactiva típica después de sembrar está por debajo del 2 %.

> **Tip — Comprobación del progreso de la semilla**
> Las líneas de registro de semillas se emiten en`Info`Nivel bajo el`🗄️ Data`Categoría OSLog. Para transmitirlos:
>```bash
> flujo de registro --predicate 'proceso == "Meshtastic" Y eventMessage CONTIENE "[PerfSeed]"' --información de nivel
>```

### Lógica de omitir y volver a sembrar

Si la tienda ya contiene al menos tantos nodos como`MESHTASTIC_PERF_SEED_NODES`Solicitudes, la siembra se omite a menos que`MESHTASTIC_PERF_RESET_STORE=true`Está listo. Esto significa que puede eliminar y reiniciar la aplicación contra el gran conjunto de datos existente sin esperar a una resierta.

