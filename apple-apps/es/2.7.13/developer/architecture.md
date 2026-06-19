
# Descripción general de la arquitectura

La aplicación Meshtastic Apple está dirigida a iOS, iPadOS y macOS (a través de Mac Catalyst). Se comunica con radios Meshtastic a través de BLE, TCP/IP y (en macOS) serie.

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

Los enlaces profundos utilizan el`meshtastic:///`Esquema de URL.`Router.route(url:)`Analizar la ruta y establece el estado de navegación apropiado. Ver[Enlaces profundos](deep-links)Para la referencia completa de la URL.

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
| `AccessoryManager+Position.swift` | Compartir la posición del GPS |
| `AccessoryManager+MQTT.swift` | Proxy MQTT |
| `AccessoryManager+TAK.swift` | Integración TAK/CoT |

Los protocolos de transporte están en`Meshtastic/Accessory/Transports/`.

## Persistencia

SwiftData es la única capa de persistencia.`PersistenceController.shared`Es dueño del`ModelContainer`. Uso de vistas`@Environment(\.modelContext)`O`@Query`. Las escrituras de fondo usan el`MeshPackets` `@ModelActor`.

Los tipos de modelo se definen con`@Model`En`Meshtastic/Model/`. La evolución del esquema utiliza`VersionedSchema`Y`SchemaMigrationPlan`En`MeshtasticSchema.swift`.

## Servicios

Los servicios de aplicación que no están vinculados a la conectividad de radio en vivo en`Meshtastic/Services/`.

| Ficha | Responsabilidad |
|------|---------------|
| `DocTranslationService.swift` | Traducción de la documentación en el dispositivo utilizando el marco de traducción de Apple (primario) con el respaldo de FoundationModels. Traduce archivos fuente de rebajas en inglés incluidos, cachés traducidos`.md`, Convierte a HTML a través de`MarkdownConverter`, Y activa la carga automática después del prefetch. iOS 26+. |
| `TranslationCache.swift` | Caché basado en archivos para traducir`.md`Contenido almacenado en el soporte de la aplicación. Rastrea los hashes de contenido para la detección de rancios y hace cumplir una política de desalojo de LRU de 50 MB por idioma. |
| `MarkdownConverter.swift` | Markdown→Convertidor HTML compatible con GFM. Admite encabezados, párrafos, listas, vallas de código, código en línea, tablas, enlaces, imágenes, paso HTML (`<picture>`,`<img>`), Llamadas de comillas en bloque (consejo/advertencia), negrita, cursiva, tachado, reglas horizontales, y`.md`→`.html`Reescritura del enlace. Elimina la materia frontal de YAML y los atributos en línea de Jekyll. |
| `DocsTranslationUploader.swift` | Se confirma automáticamente la traducción`.md`Archivos para`meshtastic/translations`Repositorio después de que se complete la precarga de fondo. Realiza comprobaciones de solo lectura contra`meshtastic/meshtastic`Y`meshtastic/translations`(Sin auth), luego confirma a través de la API de contenido de GitHub usando un PAT de grano fino de`Secrets.json`. El seguimiento por archivo permite volver a intentar las cargas fallidas. |
| `CommunityTranslationFetcher.swift` | Descarga las traducciones de la comunidad existentes desde el feed CDN de GitHub Pages (`index.json`) Antes de volver a la traducción en el dispositivo. Atros`nav-labels.json`Y`search-index.json`Para cadenas de interfaz de usuario traducidas y palabras clave de búsqueda. Construye una carpeta traducida pre-renderizada para que`DocBundle`Puede cargar páginas traducidas directamente. |

## Protobufs

El`MeshtasticProtobufs`Paquete Swift (`MeshtasticProtobufs/Package.swift`) Envuelve fuentes Swift generadas por protobuf. Regenerar con`./scripts/gen_protos.sh`Después de actualizar el`protobufs/`Submódulo.

