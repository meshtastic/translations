
# Unidades, medición y configuración regional

La aplicación Meshtastic muestra automáticamente temperaturas, distancias, velocidades y tiempos en las unidades que su dispositivo está configurado para usar, sin ajustes que cambiar dentro de la aplicación.

---

## Cómo funciona

Las radios Meshtastic siempre transmiten datos en **unidades métricas** (metros, °C, km/h, hPa, etc.). Cuando la aplicación recibe estos datos, los entrega al sistema de formato integrado de su dispositivo, que convierte y muestra los valores en cualquier sistema de unidades que haya elegido en **Configuración → General → Idioma y Región**.

![Language & Region settings](../assets/screenshots/settingsLanguageRegion.png)

La pantalla **Idioma y región** controla cómo la aplicación Meshtastic muestra temperaturas, distancias, fechas, números y más. Ajustes clave:

| Escenario | Lo que controla en Meshtastic |
|---|---|
| **Temperatura** | °C o °F para todas las lecturas del sensor y el clima |
| **Sistema de medición** | Métrico (m, km, kg, mm) o EE. UU./Reino Unido (ft, mi, lbs, in) |
| **Calendario** | Sistema de calendario para todas las fechas |
| **Primer día de la semana** | El día de inicio de la semana se muestra en la fecha |
| **Formato de fecha** | Fecha del pedido en toda la aplicación |
| **Formato de número** | Separadores decimales y agrupación de dígitos |

> **Tip — Nunca necesitas alternar unidades dentro de la aplicación. ** Cambie las preferencias de medición de su sistema y cada pantalla en Meshtastic se actualiza automáticamente: detalles de nodos, gráficos de telemetría, clima, altitud y más.

## Temperatura

Los valores de temperatura de los sensores ambientales y los pronósticos meteorológicos se transmiten como **°C** y se muestran como **°C** o **°F** según la preferencia de la unidad de temperatura de su dispositivo.

| Tu configuración | Ves |
|---|---|
| Grados Celsius | 22 °C |
| Fahrenheit | 72 °F |

Esto afecta a todas las pantallas de temperatura en toda la aplicación: telemetría del entorno del nodo, temperatura del suelo, punto de rocho, pronósticos meteorológicos y ejes de gráficos de telemetría.

## Distancia y altitud

Las distancias entre los nodos y las altitudes GPS se transmiten como **metros** y se escalan y convierten automáticamente por el sistema.

| Tu configuración | Distancia corta | Gran distancia | Altitud |
|---|---|---|---|
| Métrico | 350 m | 2,5 km | 1.200 m |
| Imperial (EE. UU.) | 1.148 pies | 1,6 millas | 3.937 pies |

La aplicación utiliza la escala natural: las distancias cortas se mantienen en metros o pies, mientras que las distancias más largas cambian automáticamente a kilómetros o millas.

### ¿Dónde aparecen estos?

- **Lista de nodos** - distancia y rumbo a cada nodo
- **Detalle del nodo** - altitud, distancia desde su posición
- **Mapa** — distancias de punto de ruta, distancias de salto de ruta de rastreo
- **Compasa** — distancia al nodo seleccionado
- **Tabla de altitud** — Las etiquetas del eje Y se adaptan a su configuración regional

## Velocidad

La velocidad del suelo del GPS se muestra en la unidad de velocidad preferida de su localidad.

| Tu configuración | Ves |
|---|---|
| Métrico | 12 km/h |
| Imperial (EE. UU.) | 7 mph |

La velocidad aparece en la pantalla **Estado del GPS** cuando su dispositivo tiene una solución de GPS activa.

## Viento

Los datos de velocidad del viento y ráfaga de los sensores ambientales se transmiten como **m/s** y se convierten para su visualización.

| Tu configuración | Ves |
|---|---|
| Métrico | 5 m/s |
| Imperial (EE. UU.) | 11 mph |

Las lecturas del viento aparecen en la sección meteorológica **Detalle del nodo** y en las columnas de registro de **Telemetría ambiental**.

## Peso

La telemetría de peso se transmite como **kg** y se convierte para la visualización.

| Tu configuración | Ves |
|---|---|
| Métrico | 24,5 kg |
| Imperial (EE. UU.) | 54,0 libras |

## Lluvia

Las mediciones de lluvia (total de 1 hora y 24 horas) se transmiten como **mm** y se convierten para su visualización.

| Tu configuración | Ves |
|---|---|
| Métrico | 12 mm |
| Imperial (EE. UU.) | 0,5 en |

## Unidades que nunca cambian

Algunas unidades son estándares internacionales y se muestran de la misma manera independientemente de su ubicación:

| Medición | Unidad | Por qué |
|---|---|---|
| Presión barométrica | HPa | Norma meteorológica internacional |
| Dirección / rodamiento | ° (grados) | Convención de navegación universal |
| Radiación | µR/hora | Unidad de dosimetría estándar |
| Coordenades GPS | Grados decimales | Estándar geográfico universal |
| Humedad, batería, humedad del suelo | % | Universal |

## Fecha y hora

Todas las marcas de tiempo en toda la aplicación (última escucha, horas de mensajes, registros de telemetría, ejes de gráficos) siguen las preferencias de fecha y hora de su dispositivo.

| Escenario | Lo Que Controla | Ejemplo |
|---|---|---|
| **Horario de 24 horas** | Formato de reloj | 14:30 frente a 2:30 p. m. |
| **Formato de fecha** | Fecha de pedido | 05/09/2026 frente a 09/05/2024 frente a 09-05-2026 |
| **Calendario** | Sistema de calendario | Gregoriano, budista, japonés, etc. |

La aplicación también utiliza **tiempo relativo** donde tiene sentido, por ejemplo, "hace 5 minutos" o "hace 2 horas" en la lista de nodos, que se localiza automáticamente en el idioma de su dispositivo.

## Cambiando su sistema de medición

Su sistema de medición (métrico frente a imperial) está vinculado a la configuración de su región. Para cambiarlo sin cambiar tu idioma:

1. Abrir **Configuración → General → Idioma y región**
2. Toque **Sistema de medición**
3. Elija **Métrico**, **EE. UU.** o **Reino Unido**

La aplicación Meshtastic recoge el cambio inmediatamente, sin necesidad de reiniciar.

> **Tip — Reino Unido vs. Imperial de EE. UU. ** El sistema de medición del Reino Unido utiliza millas para la distancia, pero piedras para el peso corporal y Celsius para la temperatura. El sistema estadounidense utiliza Fahrenheit y libras. La aplicación respeta estas distinciones automáticamente.

