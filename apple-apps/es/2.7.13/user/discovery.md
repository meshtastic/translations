
# Descubrimiento de malla local

Local Mesh Discovery escanea su área para encontrar radios Meshtastic cercanas que funcionan con diferentes configuraciones de frecuencia. Úselo para identificar qué configuración de LoRa está más activa en su ubicación antes de configurar un nuevo nodo.

## Lo que hace

El escáner cambia a través de una serie de preajustes de módem LoRa, escucha un período establecido en cada uno y registra cuántos nodos escucha y qué tan ocupadas están las ondas de radio (utilización del canal). A continuación, presenta una lista clasificada de configuraciones ordenadas por actividad.

Cada preajuste se escanea en la **ranura de frecuencia predeterminada** para que la radio escuche en la misma frecuencia que utiliza la malla pública. Si su radio está configurada en una ranura de frecuencia personalizada, el escaneo utiliza temporalmente la ranura predeterminada mientras se ejecuta y restaura su configuración LoRa original, incluida su ranura de frecuencia, automáticamente cuando finaliza el escaneo.

En los dispositivos compatibles con iOS 26+, el asistente de IA en el dispositivo analiza los resultados del escaneo y recomienda la mejor configuración para su ubicación, sin necesidad de conexión a Internet.

## Ejecutando un escaneo

![Radar sweep active — scan in progress](../assets/screenshots/radarActive.png)

1. Vaya a **Configuración → Local Mesh Discovery**.
2. Toque **Iniciar escaneo**.
3. El escáner recorre la configuración automáticamente. Cada ciclo dura unos minutos; no cierres la aplicación durante un escaneo.
4. Cuando se completa el análisis, los resultados aparecen clasificados por número de nodos y actividad del canal.

## Resultados de lectura

![Discovery results summary with two presets](../assets/screenshots/summaryTwoPresets.png)

| Columna | Descripción |
|--------|-------------|
| Programar con anterioridad | Preajuste del módem LoRa (por ejemplo, largo rápido, largo lento) |
| Nodos Escuchados | Número de nodos distintos detectados en esta configuración |
| Utilización del canal | Porcentaje de tiempo de emisión utilizado - mayor significa más activo |
| Recomendación | ✅ La mejor combinación para tu área |

## Aplicando una configuración

Toque una fila de resultados y luego **Aplicar configuración** para configurar su radio conectada para que coincida con la configuración más activa en su área. Esto actualiza la configuración de LoRa en la radio directamente.

---

> **Tip — ¿Qué hace esto?**
> Esta herramienta escanea su área local para encontrar radios Meshtastic cercanas en diferentes configuraciones de frecuencia. Cambia entre configuraciones automáticamente, escucha durante unos minutos en cada una y luego le muestra qué configuración funciona mejor para su ubicación en función de cuántas radios encuentra y qué tan ocupadas están las ondas de radio. En los dispositivos compatibles, la IA local en el dispositivo analizará los resultados de su escaneo y recomendará la mejor configuración, sin necesidad de conexión a Internet.

