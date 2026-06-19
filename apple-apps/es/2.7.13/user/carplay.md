
# CarPlay

La aplicación es compatible con Apple CarPlay para **mensajería de malla manos libres** mientras se conduce. La interfaz de CarPlay se integra con el sistema de mensajería iOS y Siri para que los usuarios puedan enviar y recibir mensajes Meshtastic sin mirar su teléfono.

## Requisitos

- IPhone con iOS 18 o posterior
- Una unidad principal de CarPlay compatible o el simulador de CarPlay en Xcode
- Un dispositivo Meshtastic conectado a través de Bluetooth, TCP o serie
- Siri habilitado: la aplicación solicita la autorización de Siri durante la incorporación y de nuevo en los lanzamientos posteriores

## Interfaz

La pantalla de CarPlay presenta una **interfaz de dos pestañas**:

| Anilla | Descripción |
|-----|-------------|
| **Canales** | Enumera todos los canales de malla activos |
| **Mensajes directos** | Lista de contactos recientes y favoritos |

Cuando ningún dispositivo Meshtastic está conectado, ambas pestañas muestran un elemento de estado **"No conectado"** con un mensaje para abrir la aplicación Meshtastic.

### Pestaña de canales

Cada fila de canales muestra:
- El nombre del canal (o "Canal principal" para el índice 0)
- Una insignia de mensaje no leído cuando hay mensajes no leídos
- "Primario" o "Ch N" como texto detallado

Al tocar una fila de canales se inicia una sesión de composición de Siri para ese canal.

### Pestaña Mensajes Directos

La pestaña Mensajes directos se divide en dos secciones:

- **Favoritos** - Nodos marcados como favoritos, ordenados por última escucha
- **Reciente** - Todos los demás contactos con mensajes con historial, ordenados por última escucha (con un límite de 24 entradas)

Cada fila de contactos muestra:
- Nombre de contacto e icono de persona
- Recuento de mensajes no leídos cuando corresponda
- Tiempo desde la última vez que se escuchó (por ejemplo, "Justo ahora", "hace 5 minutos", "hace 2 horas", "hace 3 días")

## Comandos de voz de Siri

Usa estos comandos de voz de Siri en CarPlay para interactuar con Meshtastic:

| Comando de voz | Frase de ejemplo | Descripción |
|---|---|---|
| Enviar mensaje | "Envía un mensaje en Meshtastic" | Redacta y envía un mensaje de texto a un contacto o canal |
| Buscar mensajes | "Buscar mensajes de Meshtastic" | Historial de mensajes de búsqueda |
| Marcar como leído | "Marcar el mensaje de Meshtastic como leído" | Marca una conversación como leída |

> **Warning — Límites de mensajes:**
> Los mensajes están limitados a **200 bytes** (UTF-8). Siri no enviará mensajes que superen este límite. Solo se admite un **un solo destinatario** por mensaje, sin mensajes directos grupales. Los mensajes solo emoji y los mensajes de administración están excluidos de CarPlay.

## Anuncios de mensajes entrantes

Cuando CarPlay está conectado y **Notificaciones de anuncio** está habilitado en Configuración de iOS → Siri, Siri lee los mensajes Meshtastic entrantes en voz alta. Solo los mensajes de texto no emoji y no administradores activan anuncios.

Hasta 50 mensajes no leídos que llegaron antes de que comenzara la sesión de CarPlay se donan a Siri en el momento de la conexión para que puedan ser leídos bajo demanda.

Después de que Siri lea los mensajes en voz alta, se marcan automáticamente como leídos para que ya no aparezcan como no leídos en la aplicación o en futuras sesiones de CarPlay.

## Actividad en vivo

Cuando un dispositivo Meshtastic se conecta durante una sesión de CarPlay, una **Isla dinámica / Actividad en vivo de la pantalla de bloqueo** se inicia automáticamente (solo iOS, no disponible en macOS). Muestra:

- Nombre del nodo y nombre corto
- Tiempo de actividad, utilización del canal y porcentaje de tiempo de emisión TX
- Estadísticas de paquetes enviados, recibidos y de retransmisión
- En línea y recuentos totales de nodos
- Un temporizador de cuenta regresiva de 15 minutos sincronizado con el intervalo de informes de telemetría

La actividad en vivo termina automáticamente cuando CarPlay se desconecta.

> **Tip —**Para obtener detalles de la implementación y la arquitectura de los componentes, consulte la [Guía para desarrolladores de CarPlay](../developer/carplay.md).


