
# Medición y configuración regional

Cómo la aplicación convierte los datos métricos del dispositivo en valores de visualización con valor de configuración regional. Todas las reglas están definidas en[Normas de diseño Meshtastic v1.4, Sección 10](https://github.com/meshtastic/design/blob/master/standards/meshtastic_design_standards_latest.md).

## Principio básico

Los dispositivos Meshtastic transmiten todos los datos en **unidades SI métricas**. La aplicación envuelve los valores brutos en Swift`Measurement`Tipos con la unidad de origen correcta, luego permite que el sistema operativo los formatee para la configuración regional del usuario. Sin manual`if metric … else imperial`La ramificación es necesaria para la mayoría de las cantidades.

```
Device (protobuf, always metric)
  → Wrap in Measurement<Unit>(value:, unit: .sourceUnit)
  → Format with .formatted(.measurement(...)) or MeasurementFormatter
  → Display (auto-converted to user's locale)
```

## Unidades de origen Protobuf

Estas son las unidades canónicas que envía el dispositivo. Utilice siempre estos como unidad de origen al construir`Measurement`Valores:

| Cantidad | Unidad de dispositivo | Tipo de unidad Swift |
|----------|------------|-----------------|
| Altitud | Metros | `UnitLength.meters` |
| Distancia (sensor) | Milímetros | `UnitLength.millimeters` |
| Velocidad en tierra | Km/h | `UnitSpeed.kilometersPerHour` |
| Velocidad del viento / ráfaga | M/s | `UnitSpeed.metersPerSecond` |
| Temperatura | °C | `UnitTemperature.celsius` |
| Presión Barométrica | HPa | `UnitPressure.hectopascals` |
| Lluvia (1h / 24h) | Mm | `UnitLength.millimeters` |
| Peso | Kg | `UnitMass.kilograms` |

> **Warning — `CLLocation.speed`Devuelve m/s, no km/h. ** Al envolver la velocidad del GPS, use`UnitSpeed.metersPerSecond`. Obtener la unidad fuente incorrecta produce conversiones silenciosamente incorrectas.

## API de formato

### `.formatted(.measurement(...))`

Preferiblemente para texto en línea. Se convierte automáticamente a la configuración regional del usuario:

```swift
let speed = Measurement(value: newLocation.speed, unit: UnitSpeed.metersPerSecond)
Text(speed.formatted(.measurement(width: .abbreviated,
    numberFormatStyle: .number.precision(.fractionLength(0)))))
// → "12 km/h" or "7 mph"
```

### `MeasurementFormatter`

Se utiliza cuando se necesita más control (por ejemplo, escala natural para distancias):

```swift
let formatter = MeasurementFormatter()
formatter.unitOptions = .naturalScale  // 500m stays "500 m", 2500m → "2.5 km"
formatter.numberFormatter.maximumFractionDigits = 1
let distance = Measurement(value: meters, unit: UnitLength.meters)
return formatter.string(from: distance)
```

### `MKDistanceFormatter`

Utilizado para distancias relacionadas con el mapa. Elige automáticamente m/km o ft/mi:

```swift
let distanceFormatter = MKDistanceFormatter()
Text(distanceFormatter.string(fromDistance: Double(meters)))
```

### Temperatura

Usa el`formattedTemperature()`Extensión en`Float`(Definido en`Meshtastic/Extensions/Float.swift`):

```swift
// Auto-converts °C → °F based on locale
Text(temperature.formattedTemperature())
```

Cuando necesite el valor convertido sin procesar (por ejemplo, para los puntos de datos del gráfico), use`localeTemperature()`:

```swift
let displayValue = temperature.localeTemperature()  // Double in user's preferred unit
```

Ambos métodos utilizan`kCFLocaleTemperatureUnitKey`Para detectar la preferencia de temperatura del usuario.

## Detección de configuración regional

### Unidad de temperatura

```swift
let locale = NSLocale.current as NSLocale
let localeUnit = locale.object(forKey: NSLocale.Key(rawValue: "kCFLocaleTemperatureUnitKey"))
if (localeUnit as? String) == "Fahrenheit" {
    // Use .fahrenheit
}
```

> **Warning — Nunca force-desenvuelva las consultas de la configuración regional.** `localeUnit`Puede ser`nil`En algunas versiones del sistema operativo. Usa siempre`as? String`Con un defecto seguro (Celsius).

### Sistema de medición

```swift
let usesMetric = Locale.current.measurementSystem == .metric
```

Utilizado para cantidades donde`Measurement`El formato no se aplica completamente (por ejemplo, elegir la precisión decimal para la precipitación: 0 decimales por mm, 1 para pulgadas).

## Unidades que nunca se convierten

Estos se muestran tal cual, independientemente de la configuración regional:

| Cantidad | Unidad | Por qué |
|----------|------|-----|
| Presión Barométrica | HPa | Norma meteorológica internacional |
| Dirección / Rodamiento | ° (grados) | Convención de navegación universal |
| Radiación | µR/hora | Unidad de dosimetría estándar |
| Coordenadas | Grados decimales | Estándar geográfico universal |
| Porcentajes (humedad, batería) | % | Universal |

## Tablas y gráficos

Los ejes de gráficos, las informacións de herramientas y las anotaciones también deben mostrar unidades con constenciales:

```swift
// Altitude chart Y-axis (PositionAltitudeChart.swift)
AxisValueLabel("""
    \(value.as(PlottableMeasurement.self)!
        .measurement
        .converted(to: Locale.current.measurementSystem == .metric
            ? .meters : .feet),
        format: .measurement(width: .wide,
            numberFormatStyle: .number.precision(.fractionLength(0))))
""")
```

## Fecha y hora

| Caso de uso | IPA | Ejemplo |
|----------|-----|---------|
| Indicadores de recurrencia | `RelativeDateTimeFormatter()` | "Hace 5 minutos" |
| Marcas de tiempo | `Date.formatted(date: .numeric, time: .shortened)` | "9/5/26, 2:30 PM" |
| Plantillas con reste de configuración regional | `DateFormatter.dateFormat(fromTemplate:options:locale:)` | Respeta 12/24 horas, orden de fecha |
| Exportar (legible por máquina) | `DateFormatter`Con`en_US_POSIX`Local | "2026-05-09_143000" |
| TAK/CoT XML | `Date.ISO8601FormatStyle` | ISO 8601 con fraccionamiento de segundos |

Nunca codifique el formato de 12 o 24 horas, deja que el sistema operativo lo maneje a través de formateadores con código de ubicación.

## Mapa de archivos

| Ficha | Lo que hace |
|------|-------------|
| `Extensions/Float.swift` | `formattedTemperature()`, `localeTemperature()` |
| `Views/Settings/GPSStatus.swift` | Formato de velocidad GPS (fuente m/s) |
| `Views/Helpers/Weather/LocalWeatherConditions.swift` | Temperatura y viento de WeatherKit |
| `Views/Helpers/Weather/NodeWeatherForecast.swift` | Conversión de temperatura prevista por hora |
| `Views/Nodes/Helpers/Map/PositionAltitudeChart.swift` | Eje del gráfico de altitud consciente de la localidad |
| `Views/Nodes/Helpers/NodeDetail.swift` | Peso, precipitación, viento, visualización de la temperatura del suelo |
| `Views/Nodes/Helpers/Metrics Columns/EnvironmentDefaultColumns.swift` | Columnas de la tabla de telemetría |
| `Views/Nodes/Helpers/Metrics Columns/EnvironmentDefaultSeries.swift` | Gráfico de umbrales de temperatura de gradiente |
| `Views/Helpers/DistanceText.swift` | `MKDistanceFormatter`Envoltura |
| `Views/Helpers/CompassView.swift` | `MeasurementFormatter` with `.naturalScale` |
| `Measurement/CustomFormatters.swift` | Compartido`altitudeFormatter` |

## Lista de verificación para nuevos campos de telemetría

Al agregar un nuevo valor de sensor o pantalla de telemetría:

1. Identifique la unidad de origen protobuf del esquema del dispositivo
2. Envolver`Measurement<Unit>(value:, unit:)`Con la unidad de origen correcta
3. Formato con`.formatted(.measurement(...))`— No codifique las cadenas de unidad
4. Si se trata de un gráfico, asegúrese de que las etiquetas de eje utilicen la misma conversión con reste de configuración regional
5. Si es una unidad universal (hPa, grados, %), muéstrela tal y como está
6. Prueba con la configuración del sistema de medición estadounidense y métrica en el simulador

