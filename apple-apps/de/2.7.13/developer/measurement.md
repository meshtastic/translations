
# Messung & Gebietsschema

Wie die App metrische Gerätedaten in standortbewusste Anzeigewerte umwandelt. Alle Regeln sind definiert in [Meshtastic Design Standards v1.4, Abschnitt 10](https://github.com/meshtastic/design/blob/master/standards/meshtastic_design_standards_latest.md).

## Kernprinzip

Meshtastic-Geräte übertragen alle Daten in **metrischen SI-Einheiten**. Die App umschließt Rohwerte in Swift`Measurement`Typen mit der richtigen Quelleinheit, dann lässt das Betriebssystem sie für das Gebietsschema des Benutzers formatieren. Kein Handbuch`if metric … else imperial`Verzweigung ist für die meisten Mengen erforderlich.

```
Device (protobuf, always metric)
  → Wrap in Measurement<Unit>(value:, unit: .sourceUnit)
  → Format with .formatted(.measurement(...)) or MeasurementFormatter
  → Display (auto-converted to user's locale)
```

## Protobuf-Quelleinheiten

Dies sind die kanonischen Einheiten, die das Gerät sendet. Verwenden Sie diese immer als Quelleinheit bei der Konstruktion`Measurement`Werte:

| Messbare Größe | Geräteeinheit | Swift-Einheitstyp |
|----------|------------|-----------------|
| Höhe | Meter | `UnitLength.meters` |
| Entfernung (Sensor) | Millimeter | `UnitLength.millimeters` |
| Grundgeschwindigkeit | Km/h | `UnitSpeed.kilometersPerHour` |
| Windgeschwindigkeit / Böen | M/s | `UnitSpeed.metersPerSecond` |
| Temperatur | °C | `UnitTemperature.celsius` |
| Luftdruck | HPa | `UnitPressure.hectopascals` |
| Niederschlag (1h / 24h) | Mm | `UnitLength.millimeters` |
| Gewicht | Kg | `UnitMass.kilograms` |

> **Warning — `CLLocation.speed`Gibt m/s zurück, nicht km/h. ** Verwenden Sie beim Umschließen der GPS-Geschwindigkeit`UnitSpeed.metersPerSecond`. Wenn Sie die Quelleinheit falsch machen, führen Sie stillschweigend zu falschen Konvertierungen.

## Formatierungs-APIs

### `.formatted(.measurement(...))`

Bevorzugt für Inline-Text. Konvertiert automatisch in das Gebietsschema des Benutzers:

```swift
let speed = Measurement(value: newLocation.speed, unit: UnitSpeed.metersPerSecond)
Text(speed.formatted(.measurement(width: .abbreviated,
    numberFormatStyle: .number.precision(.fractionLength(0)))))
// → "12 km/h" or "7 mph"
```

### `MeasurementFormatter`

Wird verwendet, wenn Sie mehr Kontrolle benötigen (z. B. natürliche Skalierung für Entfernungen):

```swift
let formatter = MeasurementFormatter()
formatter.unitOptions = .naturalScale  // 500m stays "500 m", 2500m → "2.5 km"
formatter.numberFormatter.maximumFractionDigits = 1
let distance = Measurement(value: meters, unit: UnitLength.meters)
return formatter.string(from: distance)
```

### `MKDistanceFormatter`

Wird für kartenbezogene Entfernungen verwendet. Nimmt automatisch m/km oder ft/mi aus:

```swift
let distanceFormatter = MKDistanceFormatter()
Text(distanceFormatter.string(fromDistance: Double(meters)))
```

### Temperatur

Verwenden Sie die`formattedTemperature()`Verlängerung auf`Float`(Definiert in`Meshtastic/Extensions/Float.swift`):

```swift
// Auto-converts °C → °F based on locale
Text(temperature.formattedTemperature())
```

Wenn Sie den rohen konvertierten Wert benötigen (z. B. für Diagrammdatenpunkte), verwenden Sie`localeTemperature()`:

```swift
let displayValue = temperature.localeTemperature()  // Double in user's preferred unit
```

Beide Methoden verwenden`kCFLocaleTemperatureUnitKey`Um die Temperaturpräferenz des Benutzers zu erkennen.

## Erkennung des Gebietsschemas

### Temperatureinheit

```swift
let locale = NSLocale.current as NSLocale
let localeUnit = locale.object(forKey: NSLocale.Key(rawValue: "kCFLocaleTemperatureUnitKey"))
if (localeUnit as? String) == "Fahrenheit" {
    // Use .fahrenheit
}
```

> **Warning — Erzwingen Sie niemals das Abwickeln von Gebietsschema-Abfragen.** `localeUnit`Kann sein`nil`Auf einigen OS-Versionen. Verwenden Sie immer`as? String`Mit einem sicheren Standard (Celsius).

### Messsystem

```swift
let usesMetric = Locale.current.measurementSystem == .metric
```

Wird für Mengen verwendet, bei denen`Measurement`Die Formatierung trifft nicht vollständig zu (z. B. die Auswahl der Dezimalpräzision für die Niederschlagsmenge: 0 Dezimalstellen für mm, 1 für Zoll).

## Einheiten, die niemals konvertieren

Diese werden unabhängig vom Gebietsschema so angezeigt, wie es ist:

| Messbare Größe | Einheit | Warum |
|----------|------|-----|
| Luftdruck | HPa | Internationale meteorologische Norm |
| Überschrift / Lager | ° (Grad) | Universelle Navigationskonvention |
| Strahlung | µR/Std. | Standard-Dosimetrieeinheit |
| Koordinaten | Dezimalgrad | Universeller geografischer Standard |
| Prozentsätze (Feuchtigkeit, Batterie) | Prozent | Universell |

## Diagramme & Grafiken

Diagrammachsen, Tooltips und Anmerkungen müssen auch gebietswahrende Einheiten anzeigen:

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

## Datum & Uhrzeit

| Anwendungsfall | Programmierschnittstelle | Beispiel |
|----------|-----|---------|
| Aktualitätsindikatoren | `RelativeDateTimeFormatter()` | "Vor 5 Minuten" |
| Zeitstempel | `Date.formatted(date: .numeric, time: .shortened)` | "9.5/26, 14:30 Uhr" |
| Gebietsschema-bewusste Vorlagen | `DateFormatter.dateFormat(fromTemplate:options:locale:)` | Respektiert 12/24 Stunden, Datumsreihenfolge |
| Exportieren (maschinenlesbar) | `DateFormatter`Mit`en_US_POSIX`Ort | "2026-05-09_143000" |
| TAK/CoT XML | `Date.ISO8601FormatStyle` | ISO 8601 mit Bruchteilen von Sekunden |

Kodieren Sie niemals ein 12-Stunden- oder 24-Stunden-Format - lassen Sie das Betriebssystem es über standortbewusste Formatierer verarbeiten.

## Dateikarte

| Dossier | Was Es Tut |
|------|-------------|
| `Extensions/Float.swift` | `formattedTemperature()`, `localeTemperature()` |
| `Views/Settings/GPSStatus.swift` | GPS-Geschwindigkeitsformatierung (m/s Quelle) |
| `Views/Helpers/Weather/LocalWeatherConditions.swift` | WeatherKit Temperatur & Wind |
| `Views/Helpers/Weather/NodeWeatherForecast.swift` | Stündliche Vorhersage Temperaturumrechnung |
| `Views/Nodes/Helpers/Map/PositionAltitudeChart.swift` | Ortsbewusste Höhendiagrammachse |
| `Views/Nodes/Helpers/NodeDetail.swift` | Gewicht, Niederschlag, Wind, Bodentemperaturanzeige |
| `Views/Nodes/Helpers/Metrics Columns/EnvironmentDefaultColumns.swift` | Telemetrietabellenspalten |
| `Views/Nodes/Helpers/Metrics Columns/EnvironmentDefaultSeries.swift` | Diagramm Gradiententemperaturschwellen |
| `Views/Helpers/DistanceText.swift` | `MKDistanceFormatter`Verpackung |
| `Views/Helpers/CompassView.swift` | `MeasurementFormatter` with `.naturalScale` |
| `Measurement/CustomFormatters.swift` | Geteilt`altitudeFormatter` |

## Checkliste für neue Telemetriefelder

Beim Hinzufügen eines neuen Sensorwerts oder einer Telemetrieanzeige:

1. Identifizieren Sie die Protobuf-Quelleinheit aus dem Geräteschema
2. Einwickeln`Measurement<Unit>(value:, unit:)`Mit der richtigen Quelleinheit
3. Formatieren mit`.formatted(.measurement(...))`- Einheitszeichenfolgen nicht fest codieren
4. Wenn es sich um ein Diagramm handelt, stellen Sie sicher, dass die Achsenbeschriftungen dieselbe gebietsbewusste Konvertierung verwenden.
5. Wenn es sich um eine universelle Einheit (hPa, Grad, %) handelt, wird es so angezeigt, wie es ist
6. Testen Sie sowohl mit US- als auch mit metrischen Messsystemeinstellungen im Simulator

