
# Telemetría y sensores

Los nodos Meshtastic pueden reportar datos de sensores a través de la malla, lo que le brinda visibilidad del entorno físico en ubicaciones remotas.

## Tipos de telemetría

| Tipo | Información |
|------|------|
| Métricas del dispositivo | Nivel de la batería, voltaje de la batería, utilización del canal, fracción de tiempo de aire |
| Estadísticas locales | Paquetes recibidos/transmitidos, paquetes retransmitidos, paquetes duplicados, malas recepciones, recuentos de nodos, nivel de ruido |
| Entorno | Temperatura (°C/°F), humedad relativa (%), presión barométrica (hPa) |
| Calidad del aire | Recuentos de partículas PM1.0, PM2.5, PM10 (µg/m³) |
| Poder | Lecturas de voltaje y corriente de los sensores de monitoreo de energía |

### Métricas del dispositivo

| Icono | Estado | Descripción |
|------|-------|-------------|
| ![Battery full](../assets/screenshots/batteryFull.png) | Lleno | La batería está bien cargada (≥80%). |
| ![Battery low](../assets/screenshots/batteryLow.png) | Mínimo | La batería está baja (≤20 %); cargue el nodo pronto. |
| ![Battery charging](../assets/screenshots/batteryCharging.png) | Cargando | El nodo está enchufado y completamente cargado. |
| ![Battery unknown](../assets/screenshots/batteryNil.png) | Incógnita | Nivel de batería no reportado por este nodo. |
| ![Battery plugged in](../assets/screenshots/batteryPluggedIn.png) | Enchufado | El nodo se alimenta a través de USB/alimentación externa. |

### Estadísticas locales

Las estadísticas locales son diagnósticos por radio reportados por el propio nodo. Ayudan a diagnosticar el tráfico de malla y las condiciones del receptor con contadores para paquetes recibidos, paquetes transmitidos, paquetes retransmitidos, paquetes duplicados, malas recepciones, paquetes cancelados, nodos en línea, nodos totales y piso de ruido.

Las lecturas del suelo de ruido se muestran en dBm cuando están disponibles. Pueden cambiar rápidamente y deben interpretarse con contexto: la dirección de la antena, la interferencia cercana y los filtros externos pueden afectar el valor mostrado.

### Calidad del aire

![IAQ Scale](../assets/screenshots/iaqScale.png)

La escala de calidad del aire interior muestra las bandas de categoría desde Excelente (verde) hasta Peligroso (marrón). La aplicación admite múltiples modos de visualización para lecturas de calidad del aire:

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/aqi_all_modes_dark.png" />
  <img src="../assets/screenshots/aqi_all_modes_light.png" alt="Air Quality Index — all display modes" />
</picture>

### Entorno

| Icono | Lectura | Descripción |
|------|---------|-------------|
| ![Humidity with dew point](../assets/screenshots/humidityWithDew.png) | Humedad (con punto de rocho) | Porcentaje de humedad relativa y temperatura calculada del punto de rocido. |
| ![Humidity without dew point](../assets/screenshots/humidityNoDew.png) | Humedad | Solo porcentaje de humedad relativa. |
| ![Pressure high](../assets/screenshots/pressureHigh.png) | Alta presión | Presión barométrica por encima de lo normal (≥1013 hPa). |
| ![Pressure low](../assets/screenshots/pressureLow.png) | Baja presión | Presión barométrica por debajo de lo normal (<1013 hPa). |

### Viento

| Gadget | Descripción |
|--------|-------------|
| ![Wind full](../assets/screenshots/windFull.png) | Velocidad del viento, velocidad de ráfaga y dirección. |
| ![Wind minimal](../assets/screenshots/windMinimal.png) | Solo velocidad del viento (sin ráfagas ni datos de dirección disponibles). |

### Radiación

| Gadget | Descripción |
|--------|-------------|
| ![Radiation](../assets/screenshots/radiation.png) | Nivel de radiación en µR/h de un sensor de contador Geiger conectado. |

## Telemetría de visualización

La telemetría es visible en dos lugares:

1. **Detalle del nodo**: toque cualquier nodo en la pestaña Nodos. La sección Registros muestra las métricas del dispositivo y las lecturas del entorno más recientes.
2. **Tablas de telemetría**: toque el icono del gráfico en un detalle de nodo para ver gráficos históricos de cualquier tipo de telemetría que el nodo haya informado.

## Configuración de la telemetría

Vaya a **Configuración → Telemetría** para habilitar los módulos de telemetría y establecer intervalos de informes:

![Telemetry Config](../assets/screenshots/telemetryConfig.png)

| Escenario | Descripción |
|---------|-------------|
| Intervalo de métricas del dispositivo | Con qué frecuencia (segundos) el nodo transmite datos de batería y utilización. |
| Intervalo de entorno | Con qué frecuencia se transmiten los datos del sensor ambiental. |
| Métricas de calidad del aire habilitadas | Habilitar o deshabilitar los informes del sensor de calidad del aire. Cuando está habilitado, aparece el selector de intervalos. |
| Intervalo de calidad del aire | ¿Con qué frecuencia se transmiten los datos del sensor de calidad del aire? El valor predeterminado es de 30 minutos. |
| Pantalla de entorno | Mostrar los datos del entorno en la pantalla del dispositivo. |
| Telemetría en el canal de administración | Restringir la telemetría al canal de administración en lugar de la transmisión. |

## Sensores compatibles

La aplicación muestra datos de cualquier sensor compatible con el firmware de Meshtastic. Sensores comunes:

- **BME280 / BME680** — temperatura, humedad, presión
- **SHT31** — temperatura, humedad
- **MCP9808** — temperatura de precisión
- **INA219 / INA260** — monitoreo de energía
- **PMSA003** — calidad del aire (PM2.5)

La disponibilidad del sensor depende de su hardware. Revisa el[Guía de hardware de Meshtastic](https://meshtastic.org/docs/hardware/)Por compatibilidad.

## Sensor de detección

El módulo de sensor de detección alerta a la malla cuando se activa un sensor de movimiento PIR conectado o un interruptor de contacto. Configúrelo en **Configuración → Sensor de detección**. Las alertas aparecen como mensajes en el canal principal y como entradas de registro de nodos.

