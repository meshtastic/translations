
# Ajustes
La pestaña Configuración le permite configurar la aplicación y su radio Meshtastic conectada.
## Configuración de la aplicación
Preferencias generales de la aplicación, incluyendo el estilo del mapa, el comportamiento de la notificación y el tema. Estos afectan solo a la aplicación, no a la radio.
## Configuración de la radio
La configuración de la radio requiere un nodo conectado. Seleccione su nodo de la sección **Configurar** si tiene varios nodos.
### Lora
La configuración de LoRa controla cómo se comunica su radio en la malla:
| Escenario | Descripción |
|---------|-------------|
| Región | Tu región geográfica. **Debe establecerse correctamente** - usar la región incorrecta es ilegal e impide la comunicación con los nodos locales. Todas las regiones definidas en el protobuf de Meshtastic están disponibles, incluyendo Nepal 865MHz, Brasil 902MHz, Región 1 Amateur 2m de la UIT, Región 2 y 3 Amateur 2m, EU 866, EU 874, EU 917 y la banda mundial de 2,4 GHz. |
| Preajuste del módem | Intercambio de velocidad/rango. La mayoría de los usuarios deberían usar Long Fast o Long Slow. |
| Límite de salto | El número de veces que otros nodos repiten un mensaje. Los valores más altos aumentan el rango, pero también el tráfico de malla. |
| Ranura de frecuencia | Afina la frecuencia exacta dentro de tu región. |

### Canales
Gestiona hasta 8 canales (0–7). El canal 0 es el canal de transmisión principal. Los canales adicionales crean grupos de mensajería aislados con sus propias claves de cifrado.
### Seguridad
Configure el cifrado PKI (Infraestructura de Clave Pública) para mensajes directos. Requiere firmware 2.5+.
### Usuario
Establezca su nombre largo (nombre para mostrar) y nombre corto (identificador de 4 caracteres/emoji que se muestran en el círculo del nodo).
### Bluetooth
La configuración de la radio BLE incluye el modo PIN y el ahorro de energía. Los cambios se aplican en el próximo reinicio de la radio.
### Dispositivo
Función del dispositivo, salida en serie, transmisión de registros de depuración e intervalo de transmisión de información de nodos.
### Exhibición
Tiempo de espera de la pantalla, carrusel automático de pantallas, pantalla abatible para orientaciones de montaje alternativas y contraste OLED.
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
| Mensajes enlatados | Accesos directos de mensajes preprogramados accesibles desde los botones del dispositivo. |
| Sensor de detección | Configure sensores de movimiento o contacto PIR. |
| Notificación externa | Alertas de timbre o LED para mensajes entrantes. |
| MQTT | Mensajes de enlace ascendente/descendente a un corredor de MQTT para puentes de Internet. |
| Prueba de rango | Pruebas de alcance automatizadas con registro de posición. |
| Contador de Pax | Recuento anónimo de tráfico peatonal a través de detección de sonda Bluetooth/Wi-Fi. |
| Tono de llamada | Melodías RTTTL personalizadas para tonos de notificación. |
| Almacenar y reenviar | Almacene paquetes para nodos que están temporalmente fuera de línea. |
| Serie | Salida serie UART para integración con otro hardware. |
| Telemetría | Informes sobre el dispositivo, el medio ambiente y el sensor de calidad del aire. |

## Actualizaciones de firmware
Compruebe y aplique actualizaciones de firmware OTA a su radio conectada directamente desde la aplicación. Consulte [Actualizaciones de firmware](firmware.md) para obtener todos los detalles.
## Traducción automática de documentación
En los dispositivos con iOS 26 o posterior, la documentación de la aplicación se traduce automáticamente al idioma de su dispositivo cuando difiere del inglés.
### Cómo funciona
- **Detección de idioma**: La aplicación lee la configuración de idioma principal de su dispositivo cada vez que abre una página de documentación.- **Traducción en el dispositivo**: Las páginas se traducen utilizando el marco de traducción en el dispositivo de Apple (iOS 26+). Si un idioma no es compatible con el marco de traducción, la aplicación vuelve al modelo Foundation en el dispositivo (solo iOS 26+).- **No se requiere red**: Después de la traducción inicial, todo el contenido está disponible sin conexión.- **Caching**: Las páginas traducidas se almacenan localmente para que se carguen instantáneamente en visitas posteriores.- **Precarga de fondo**: Después de traducir la página actual, las páginas restantes se traducen en segundo plano con baja prioridad.
### Fallback al inglés
Si la traducción no está disponible (más antiguo que iOS 26, idioma no compatible o paquete de idiomas no descargado), se muestra la documentación original en inglés. La aplicación nunca muestra páginas en blanco o rotas.
### Gestión de caché
- Los archivos traducidos se almacenan en el soporte de aplicaciones y persisten en los lanzamientos de aplicaciones.- Se aplica un límite de 50 MB por idioma utilizando el desalojo menos utilizado recientemente.- Cuando se actualiza la documentación de origen en inglés (nueva versión de la aplicación), las traducciones obsteas se regeneran automáticamente.
> **Tip — Cambio de idioma**: Si cambia el idioma de su dispositivo mientras la aplicación está abierta, las páginas de documentación se recargan automáticamente en el nuevo idioma.
