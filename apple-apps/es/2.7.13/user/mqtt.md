
# MQTT

El módulo MQTT permite a un nodo Meshtastic conectar el tráfico de malla a un corredor MQTT, extendiendo la malla a través de Internet y permitiendo la integración con sistemas de automatización del hogar.

## Lo que hace MQTT

Un nodo con MQTT habilitado actúa como una puerta de enlace: publica paquetes de malla recibidos a un corredor MQTT y opcionalmente se suscribe a un tema para que los nodos remotos puedan inyectar paquetes de nuevo en la malla local.

| Icono | Estado | Descripción |
|------|-------|-------------|
| ![MQTT connected](../assets/screenshots/mqttConnected.png) | Relacionado | El puente MQTT está activo: el enlace ascendente y el enlace descendente están habilitados. |
| ![MQTT uplink only](../assets/screenshots/mqttUplinkOnly.png) | Solo enlace ascendente | Publicar paquetes de malla al corredor, pero no suscribirse a los paquetes entrantes. |
| ![MQTT disconnected](../assets/screenshots/mqttDisconnected.png) | Desconectado | MQTT está configurado, pero actualmente no está conectado al corredor. |

Esto permite que dos redes de malla en diferentes ubicaciones físicas aparezcan como una red lógica, siempre y cuando al menos un nodo en cada ubicación tenga acceso a Internet.

## Configuración de MQTT

Ir a **Configuración → MQTT**:

![MQTT Config](../assets/screenshots/mqttConfig.png)

| Escenario | Descripción |
|---------|-------------|
| Servidor MQTT | Nombre de host o IP de su corredor MQTT (por ejemplo, `mqtt.meshtastic.org` para el corredor público). |
| Puerto | El valor predeterminado es 1883 (sin cifrar) o 8883 (TLS). |
| Nombre de usuario | Nombre de usuario del corredor MQTT (opcional). |
| Contraseña | Contraseña del corredor MQTT (opcional). |
| Tema raíz | El prefijo de tema para todos los mensajes publicados (por defecto: `msh`). |
| Habilitado | Activar/desactar el puente MQTT. |
| Cifrado habilitado | Cifrar los paquetes antes de publicarlos. Recomendado: evita que el corredor lea el contenido del mensaje. |
| JSON habilitado | Publicar paquetes JSON decodificados además del formato binario protobuf. Útil para integraciones de automatización del hogar. |
| TLS habilitado | Utilice TLS para la conexión MQTT. Requiere un corredor con soporte TLS. |
| Proxy al cliente | Enrute el tráfico MQTT a través de la aplicación del teléfono en lugar de directamente desde la radio. Útil para radios sin Wi-Fi. |

## Estructura del tema

Meshtastic publica para:

```
<root_topic>/<region>/<channel_index>/<node_id>/<packet_type>
```

Ejemplo:`msh/US/2/!a1b2c3d4/text`

## Consideraciones de seguridad

- Habilitar MQTT con una ubicación de transmisión de canal inseguro y mensajes a Internet.
- El indicador de seguridad del canal muestra **Inseguro con MQTT** (🔓⚠️) cuando un canal no está cifrado y MQTT está activo.
- Utilice siempre **Cifrado habilitado** en producción para proteger el contenido del mensaje.
- Considere usar un corredor privado en lugar de el público`mqtt.meshtastic.org`.

## Corredor público

El corredor público de MQTT en`mqtt.meshtastic.org`Está disponible para pruebas. **No transmita información confidencial a través del corredor público. ** Úselo solo para la verificación inicial de la configuración.

