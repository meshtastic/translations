
# Descripción general de la arquitectura

La aplicación Meshtastic Apple está dirigida a iOS, iPadOS, macOS (a través de Mac Catalyst), watchOS y visionOS. Se comunica con radios Meshtastic a través de BLE, TCP/IP y (en macOS) serie.

## Punto de entrada de la aplicación

`Meshtastic/MeshtasticApp.swift`Es el`@main` `App`Estructura. En el lanzamiento:

1. Crea`PersistenceController.shared`(Datos rápidos`ModelContainer`)
2. Instancias`AppState`(Envolturas`Router`)
3. Instancias`AccessoryManager`(BLE/TCP/conectividad en serie)
4. Instancias`AccessoryManager.shared`Como un`@EnvironmentObject`Para la jerarquía de vistas

`MeshtasticAppDelegate.swift`Mangos`UIApplicationDelegate`Ganchos para intenciones de mensajería de SiriKit CarPlay.

## Enrutador y navegación

`Router`(`Meshtastic/Router/Router.swift`) Es un`@MainActor` `ObservableObject`Que posee un`NavigationState`Estructura. Impulsa la selección de pestañas y el enrutamiento de enlaces profundos.

```
Router
└── NavigationState
    ├── MessagesNavigationState   (tab 0)
    ├── MapNavigationState        (tab 1)
    ├── NodesNavigationState      (tab 2)
    └── SettingsNavigationState   (tab 3)
```

Los enlaces profundos utilizan el`meshtastic:///`Esquema de URL.`Router.route(url:)`Analizar la ruta y establece el estado de navegación apropiado. Consulte [Enlaces profundos](enlaces profundos) para la referencia completa de la URL.

## Estado de la aplicación

`AppState`Envolturas`Router`Y se inyecta como un`@EnvironmentObject`En la raíz de la jerarquía de vistas de SwiftUI. Vistas que necesitan navegar por la lectura programática`@EnvironmentObject var router: Router`Directamente, o más comúnmente`@EnvironmentObject var appState: AppState`Y acceso`appState.router`.

## Administrador de accesorios

`AccessoryManager`Es el administrador de conectividad central dividido entre archivos de extensión:

| Ficha | Responsabilidad |
|------|---------------|
| `AccessoryManager+Discovery.swift` | Escaneo BLE, descubrimiento de dispositivos |
| `AccessoryManager+Connect.swift` | Ciclo de vida de la conexión, lógica de reconexión |
| `AccessoryManager+ToRadio.swift` | Paquetes enviados a la radio |
| `AccessoryManager+FromRadio.swift` | Paquetes recibidos de la radio |
| `Gestor de accesorios+Posición.swift` | Compartir la posición del GPS |
| `Administrador de accesorios+MQTT.swift` | Proxy MQTT |
| `Gestor de accesorios+TAK.swift` | Integración TAK/CoT |

Los protocolos de transporte están en`Meshtastic/Accessory/Transports/`.

## Persistencia

SwiftData es la única capa de persistencia.`PersistenceController.shared`Es dueño del`ModelContainer`. Uso de vistas`@Environment(\.modelContext)`O`@Query`. Las escrituras de fondo usan el`MeshPackets` `@ModelActor`.

Los tipos de modelo se definen con`@Model`En`Meshtastic/Model/`. La evolución del esquema utiliza`VersionedSchema`Y`SchemaMigrationPlan`En`MeshtasticSchema.swift`.

## Servicios

Los servicios de aplicación que no están vinculados a la conectividad de radio en vivo en`Meshtastic/Services/`.

| Ficha | Responsabilidad |
|------|---------------|
| `DocTranslationService.swift` | Traducción de la documentación en el dispositivo utilizando el marco de traducción de Apple (primario) con el respaldo de FoundationModels. Traduce los archivos fuente de rebaja en inglés empaquetados, almacena en caché traducido `.md`, convierte a HTML a través de `MarkdownConverter` y activa la carga automática después del prefetch. iOS 26+. |
| `TranslationCache.swift` | Caché basado en archivos para el contenido `.md` traducido almacenado en el soporte de aplicaciones. Rastrea los hashes de contenido para la detección de rancios y hace cumplir una política de desalojo de LRU de 50 MB por idioma. |
| `MarkdownConverter.swift` | Markdown→Convertidor HTML compatible con GFM. Admite encabezados, párrafos, listas, vallas de código, código en línea, tablas, enlaces, imágenes, paso HTML (`<image>`, `<img>`), llamadas de comillas en bloque (consejo/advertencia), negrita, cursiva, tachado, reglas horizontales y `.md` → `.html`. Elimina la materia frontal de YAML y los atributos en línea de Jekyll. |
| `DocsTranslationUploader.swift` | Comite automáticamente los archivos `.md` traducidos al repositorio `meshtastic/translations` después de que se complete la precompción en segundo plano. Realiza comprobaciones de solo lectura contra `meshtastic/meshtastic` y `meshtastic/translations` (sin auth), luego se compromete a través de la API de contenido de GitHub utilizando un PAT de grano fino de `Secrets.json`. El seguimiento por archivo permite volver a intentar las cargas fallidas. |

## Protobufs

El`MeshtasticProtobufs`Paquete Swift (`MeshtasticProtobufs/Package.swift`) Envuelve fuentes Swift generadas por protobuf. Regenerar con`./scripts/gen_protos.sh`Después de actualizar el`protobufs/`Submódulo.

