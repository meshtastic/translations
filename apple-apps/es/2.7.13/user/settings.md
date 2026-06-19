
# Ajustes

La pestaña Configuración le permite configurar la aplicación y su radio Meshtastic conectada.

## Configuración de la aplicación

Preferencias generales de la aplicación, incluyendo el estilo del mapa, el comportamiento de la notificación y el tema. Estos afectan solo a la aplicación, no a la radio.

### Gestión de datos

- **Borrar todos los datos de la aplicación** - borra la base de datos local, la caché de traducción y toda la configuración almacenada, y luego vuelve a cargar inmediatamente el catálogo de hardware del dispositivo incluido. Usa esto como último recurso.
- **Restablecimiento de NodeDB** — restablece la base de datos de nodos en su radio conectada. Cuando se le solicite, puede elegir **Preservar favoritos** para que sus nodos con estrellas se conserven después del reinicio.
- **Restablecer la configuración de la aplicación** - restaura las preferencias predeterminadas de la aplicación sin afectar a su base de datos de nodos.

## Configuración de la radio

La configuración de la radio requiere un nodo conectado. Seleccione su nodo de la sección **Configurar** si tiene varios nodos.

### Lora

La configuración de LoRa controla cómo se comunica su radio en la malla:

| Escenario | Descripción |
|---------|-------------|
| Región | Tu región geográfica. **Debe establecerse correctamente** - usar la región incorrecta es ilegal e impide la comunicación con los nodos locales. Las regiones disponibles incluyen el conjunto estándar más Nepal 865MHz, Brasil 902MHz, la ITU Región 1 Amateur 2m, ITU Región 2/3 Amateur 2m, y las bandas estrechas EU 866 / / 874 / 917 / 868. |
| Preajuste del módem | Intercambio de velocidad/rango. La mayoría de los usuarios deberían usar Long Fast o Long Slow. |
| Límite de salto | El número de veces que otros nodos repiten un mensaje. Los valores más altos aumentan el rango, pero también el tráfico de malla. |
| Ranura de frecuencia | Afina la frecuencia exacta dentro de tu región. |

### Canal

Gestiona hasta 8 canales (0–7). El canal 0 es el canal de transmisión principal. Los canales adicionales crean grupos de mensajería aislados con sus propias claves de cifrado.

### Seguridad

Configure el cifrado PKI (Infraestructura de Clave Pública) para mensajes directos. Requiere firmware 2.5+.

### Usuario

Establezca su nombre largo (nombre para mostrar) y nombre corto (identificador de 4 caracteres/emoji que se muestran en el círculo del nodo).

### Bluetooth

La configuración de la radio BLE incluye el modo PIN y el ahorro de energía. Los cambios se aplican en el próximo reinicio de la radio.

### Dispositivo

Función del dispositivo, salida en serie, transmisión de registros de depuración e intervalo de transmisión de información de nodos.

### Pantalla

Tiempo de espera de la pantalla, carrusel automático de pantallas, pantalla abatible para orientaciones de montaje alternativas y contraste OLED.

#### Orientación de la brújula

Controla en qué dirección apunta la brújula del dispositivo cuando la pantalla está en reposo. Úselo cuando su radio esté montada en ángulo o boca abajo.

| Alternativa | Descripción |
|--------|-------------|
| 0° | Orientación predeterminada: norte en la parte superior. |
| 90 °C | Girado 90° en el sentido de las agujas del reloj. |
| 180° | Girado 180° (al revés). |
| 270° | Girado 270° en el sentido de las agujas del reloj (90° en sentido contrario a las agujas del reloj). |
| 0° Invertido | Orientación predeterminada con la pantalla invertida (esperada). |
| 90° Invertido | 90° en el sentido de las agujas del reloj con la pantalla invertida. |
| 180° Invertido | 180° con la pantalla invertida. |
| 270° Invertido | 270° en el sentido de las agujas del reloj con la pantalla invertida. |

### Red

Wi-Fi SSID/contraseña para conexión TCP, servidor NTP y Ethernet (solo hardware compatible).

### Posición

Intervalo de actualización de GPS, precisión de posición y transmisión de posición inteligente. Habilite **Posición de transmisión** para compartir su ubicación con la malla.

### Poder

Perfiles de ahorro de batería, modos de suspensión y tiempo mínimo de activación. Crítico para los nodos de enrutador con energía solar.

## Configuración del módulo

Módulos de características opcionales. Solo está disponible cuando su nodo conectado es compatible con el módulo.

| Circuito | Descripción |
|--------|-------------|
| Iluminación ambiental | Controle la iluminación NeoPixel/LED en el hardware compatible. |
| Audio | Configuración de comunicación de voz Codec2. Solo disponible cuando la región de LoRa está configurada en **LORA_24** (2,4 GHz). Configure la codificación Codec2, la velocidad de bits, el pin PTT y los pines I2S GPIO. |
| Mensajes enlatados | Accesos directos de mensajes preprogramados accesibles desde los botones del dispositivo. |
| Sensor de detección | Configure sensores de movimiento o contacto PIR. |
| Notificación externa | Alertas de timbre o LED para mensajes entrantes. |
| MQTT | Mensajes de enlace ascendente/descendente a un corredor de MQTT para puentes de Internet. |
| Información del vecino | Transmite periódicamente información sobre vecinos escuchados directamente para ayudar a visualizar la topología de la malla. El intervalo de actualización varía de 4 horas (por defecto) a 72 horas. Habilite **Transmitir a través de LoRa** para compartir datos de vecinos a través de la radio además de MQTT y PhoneAPI. |
| Prueba de rango | Pruebas de alcance automatizadas con registro de posición. |
| Contador de Pax | Recuento anónimo de tráfico peatonal a través de detección de sonda Bluetooth/Wi-Fi. Configure el umbral WiFi (dBm) y el umbral BLE (dBm) para controlar la sensibilidad RSSI para el recuento de dispositivos; el valor predeterminado es -80 dBm para ambos. |
| Tono de llamada | Melodías RTTTL personalizadas para tonos de notificación. |
| Almacenar y reenviar | Almacene paquetes para nodos que están temporalmente fuera de línea. |
| Serie | Salida serie UART para integración con otro hardware. |
| Mensaje de estado | Establezca un mensaje de estado personalizado transmitido a la malla. |
| Telemetría | Informes sobre el dispositivo, el medio ambiente y el sensor de calidad del aire. |
| Gestión del tráfico | Optimización del tráfico de malla: deduplicación, limitación de velocidad y gestión de salto. Requiere firmware 2.8.0+. |

### Gestión del tráfico

El módulo de Gestión de Tráfico ayuda a reducir el tráfico de malla innecesario y a mejorar la eficiencia de la red. Está disponible en nodos que ejecutan firmware **2.8.0 o posterior**.

| Escenario | Descripción |
|---------|-------------|
| Habilitado | Habilitación maestra para el módulo de gestión de tráfico. |
| **Deduplicación de posición** |  |
| Posición Dedup | Deja caer las transmisiones de posición redundante desde el mismo nodo. |
| Bits de precisión | Número de bits de precisión para la deduplicación de posición (0-32). Los valores más bajos fusionan más posiciones. |
| Intervalo mínimo (s) | Segundos mínimos entre actualizaciones de posición desde el mismo nodo. |
| **Respuesta directa de NodeInfo** |  |
| Respuesta directa | Responda a las solicitudes de NodeInfo directamente desde la caché local en lugar de inundar la malla. |
| Max Hops | Distancia mínima de salto del solicitante antes de responder a las solicitudes de NodeInfo. |
| **Limitación de la tasa** |  |
| Limitación de la tasa | Habilite la limitación de la velocidad por nodo para acelerar los nodos de chat. |
| Ventana(s) | Ventana de tiempo en segundos para los cálculos de limitación de velocidad. |
| Paquetes máximos | Paquetes máximos permitidos por nodo dentro de la ventana de límite de velocidad. |
| **Manejo de paquetes desconocidos** |  |
| Caída desconocida | Habilitar la caída de paquetes desconocidos/indescifrables. |
| Umbral | Número de paquetes desconocidos recibidos de un nodo antes de caer. |
| **Gestión de salto** |  |
| Telemetría de salto de escape | Establezca hop_limit en 0 para las transmisiones de telemetría retransmitidas (paquetes propios no afectados). |
| Posición del salto de escape | Establezca hop_limit en 0 para las transmisiones de posición retransmitida (paquetes propios no afectados). |
| Enrutador Conserva Lúpulo | Conservar hop_limit para el tráfico de router a router. |

## Actualizaciones de firmware

Compruebe y aplique actualizaciones de firmware OTA a su radio conectada directamente desde la aplicación. Ver[Actualizaciones de firmware](firmware.md)Para todos los detalles.

## Traducción automática de documentación

En los dispositivos con iOS 26 o posterior, la documentación de la aplicación se traduce automáticamente al idioma de su dispositivo cuando difiere del inglés.

### Cómo funciona

- **Detección de idioma**: La aplicación lee la configuración de idioma principal de su dispositivo cada vez que abre una página de documentación.
- **Traducción en el dispositivo**: Las páginas se traducen utilizando el marco de traducción en el dispositivo de Apple (iOS 26+). Si un idioma no es compatible con el marco de traducción, la aplicación vuelve al modelo Foundation en el dispositivo (solo iOS 26+).
- **No se requiere red**: Después de la traducción inicial, todo el contenido está disponible sin conexión.
- **Caching**: Las páginas traducidas se almacenan localmente para que se carguen instantáneamente en visitas posteriores.
- **Precarga de fondo**: Después de traducir la página actual, las páginas restantes se traducen en segundo plano con baja prioridad.

### Fallback al inglés

Si la traducción no está disponible (más antiguo que iOS 26, idioma no compatible o paquete de idiomas no descargado), se muestra la documentación original en inglés. La aplicación nunca muestra páginas en blanco o rotas.

### Gestión de caché

- Los archivos traducidos se almacenan en el soporte de aplicaciones y persisten en los lanzamientos de aplicaciones.
- Se aplica un límite de 50 MB por idioma utilizando el desalojo menos utilizado recientemente.
- Cuando se actualiza la documentación de origen en inglés (nueva versión de la aplicación), las traducciones obsteas se regeneran automáticamente.

> **Tip — Cambio de idioma**
> Si cambia el idioma de su dispositivo mientras la aplicación está abierta, las páginas de documentación se recargan automáticamente en el nuevo idioma.

