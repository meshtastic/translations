
# Mesure et paramètres régionaux

Comment l'application convertit les données de l'appareil métrique en valeurs d'affichage connaissant les paramètres régionaux. Toutes les règles sont définies dans [Meshtastic Design Standards v1.4, Section 10](https://github.com/meshtastic/design/blob/master/standards/meshtastic_design_standards_latest.md).

## Principe fondamental

Les appareils Meshtastic transmettent toutes les données en **unités SI métriques**. L'application enveloppe les valeurs brutes dans Swift`Measurement`Types avec l'unité source correcte, puis laisse le système d'exploitation les formater pour les paramètres régionaux de l'utilisateur. Pas de manuel`if metric … else imperial`La ramification est nécessaire pour la plupart des quantités.

```
Device (protobuf, always metric)
  → Wrap in Measurement<Unit>(value:, unit: .sourceUnit)
  → Format with .formatted(.measurement(...)) or MeasurementFormatter
  → Display (auto-converted to user's locale)
```

## Unités sources Protobuf

Ce sont les unités canoniques que l'appareil envoie. Utilisez-les toujours comme unité source lors de la construction`Measurement`Valeurs :

| Quantité | Unité d'appareil | Type d'unité Swift |
|----------|------------|-----------------|
| Altitude | Mètres | `UnitLength.meters` |
| Distance (capteur) | Millimètres | `UnitLength.millimeters` |
| Vitesse au sol | Km/h | `UnitSpeed.kilometersPerHour` |
| Vitesse du vent / Rafale | M/s | `UnitSpeed.metersPerSecond` |
| Température | °C | `UnitTemperature.celsius` |
| Pression barométrique | HPa | `UnitPressure.hectopascals` |
| Précipité (1h / 24h) | Mm | `UnitLength.millimeters` |
| Poids | Kg | `UnitMass.kilograms` |

> **Warning — `CLLocation.speed`Retourne m/s, pas km/h. ** Lors de l'emballage de la vitesse GPS, utilisez`UnitSpeed.metersPerSecond`. L'erreur de l'unité source produit des conversions silencieusement incorrectes.

## API de formatage

### `.formatted(.measurement(...))`

Préféré pour le texte en ligne. Convertit automatiquement en paramètres régionaux de l'utilisateur :

```swift
let speed = Measurement(value: newLocation.speed, unit: UnitSpeed.metersPerSecond)
Text(speed.formatted(.measurement(width: .abbreviated,
    numberFormatStyle: .number.precision(.fractionLength(0)))))
// → "12 km/h" or "7 mph"
```

### `MeasurementFormatter`

Utilisé lorsque vous avez besoin de plus de contrôle (par exemple, mise à l'échelle naturelle pour les distances) :

```swift
let formatter = MeasurementFormatter()
formatter.unitOptions = .naturalScale  // 500m stays "500 m", 2500m → "2.5 km"
formatter.numberFormatter.maximumFractionDigits = 1
let distance = Measurement(value: meters, unit: UnitLength.meters)
return formatter.string(from: distance)
```

### `MKDistanceFormatter`

Utilisé pour les distances liées à la carte. Chire automatiquement m/km ou ft/mi :

```swift
let distanceFormatter = MKDistanceFormatter()
Text(distanceFormatter.string(fromDistance: Double(meters)))
```

### Température

Utilisez le`formattedTemperature()`Extension sur`Float`(Défini dans`Meshtastic/Extensions/Float.swift`) :

```swift
// Auto-converts °C → °F based on locale
Text(temperature.formattedTemperature())
```

Lorsque vous avez besoin de la valeur convertie brute (par exemple, pour les points de données du graphique), utilisez`localeTemperature()`:

```swift
let displayValue = temperature.localeTemperature()  // Double in user's preferred unit
```

Les deux méthodes utilisent`kCFLocaleTemperatureUnitKey`Pour détecter la préférence de température de l'utilisateur.

## Détection Locale

### Unité de température

```swift
let locale = NSLocale.current as NSLocale
let localeUnit = locale.object(forKey: NSLocale.Key(rawValue: "kCFLocaleTemperatureUnitKey"))
if (localeUnit as? String) == "Fahrenheit" {
    // Use .fahrenheit
}
```

> **Warning — Ne forcez jamais les requêtes de localisation.** `localeUnit`Peut être`nil`Sur certaines versions du système d'exploitation. Utilise toujours`as? String`Avec un défaut sûr (Celsius).

### Système de mesure

```swift
let usesMetric = Locale.current.measurementSystem == .metric
```

Utilisé pour les quantités où`Measurement`Le formatage ne s'applique pas entièrement (par exemple, choisir la précision décimale pour les précipitations : 0 décimale pour mm, 1 pour les pouces).

## Unités qui ne se convertissent jamais

Ceux-ci sont affichés tels quels que soit les paramètres régionaux :

| Quantité | Unité | Pourquoi |
|----------|------|-----|
| Pression barométrique | HPa | Norme météorologique internationale |
| Direction / Roulement | ° (degrés) | Convention de navigation universelle |
| Rayonnement | µR/h | Unité de dosimétrie standard |
| Coordonnées | Degrés décimaux | Norme géographique universelle |
| Pourcentages (humidité, batterie) | % | Universel |

## Tableaux et graphiques

Les axes de graphique, les info-bulles et les annotations doivent également afficher des unités de localisation :

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

## Date et heure

| Cas d'utilisation | API | Exemple |
|----------|-----|---------|
| Indicateurs de récence | `RelativeDateTimeFormatter()` | "Il y a 5 minutes" |
| Horodatages | `Date.formatted(date: .numeric, time: .shortened)` | "5/9/26, 14h30" |
| Modèles de paramètres régionaux | `DateFormatter.dateFormat(fromTemplate:options:locale:)` | Respecte 12/24h, ordre de date |
| Exporter (lisible par machine) | `DateFormatter`Avec`en_US_POSIX`Local | "2026-05-09_143000" |
| TAK/CoT XML | `Date.ISO8601FormatStyle` | ISO 8601 avec fraction de secondes |

Ne codez jamais au format 12 heures ou 24 heures - laissez le système d'exploitation le gérer via des formateurs de paramètres régionaux.

## Carte de fichiers

| Fichier | Ce qu'il fait |
|------|-------------|
| `Extensions/Float.swift` | `formattedTemperature()`, `localeTemperature()` |
| `Views/Settings/GPSStatus.swift` | Formatage de la vitesse GPS (source m/s) |
| `Views/Helpers/Weather/LocalWeatherConditions.swift` | Température et vent de WeatherKit |
| `Views/Helpers/Weather/NodeWeatherForecast.swift` | Conversion de la température des prévisions horaires |
| `Views/Nodes/Helpers/Map/PositionAltitudeChart.swift` | Axe de carte d'altitude conscient de la région |
| `Views/Nodes/Helpers/NodeDetail.swift` | Affichage du poids, des précipitations, du vent, de la température du sol |
| `Views/Nodes/Helpers/Metrics Columns/EnvironmentDefaultColumns.swift` | Colonnes de table de télémétrie |
| `Views/Nodes/Helpers/Metrics Columns/EnvironmentDefaultSeries.swift` | Graphique des seuils de température du gradient |
| `Views/Helpers/DistanceText.swift` | `MKDistanceFormatter`Emballage |
| `Views/Helpers/CompassView.swift` | `MeasurementFormatter` with `.naturalScale` |
| `Measurement/CustomFormatters.swift` | Partagé`altitudeFormatter` |

## Liste de contrôle pour les nouveaux champs de télémétrie

Lors de l'ajout d'une nouvelle valeur de capteur ou d'un écran de télémétrie :

1. Identifier l'unité source protobuf à partir du schéma du dispositif
2. Envelopper`Measurement<Unit>(value:, unit:)`Avec la bonne unité source
3. Format avec`.formatted(.measurement(...))`- Ne code pas de chaînes d'unité
4. S'il s'agirt d'un graphique, assurez-vous que les étiquettes d'axe utilisent la même conversion de localisation
5. S'il s'agit d'une unité universelle (hPa, degrés, %), affichez-la telle quelle
6. Testez avec les paramètres du système de mesure américain et métrique dans le simulateur

