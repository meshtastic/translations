
# Mesure et région
Comment l'application convertit les données de l'appareil métrique en valeurs d'affichage connaissant les paramètres régionaux. Toutes les règles sont définies dans [Meshtastic Design Standards v1.4, Section 10](https://github.com/meshtastic/design/blob/master/standards/meshtastic_design_standards_latest.md).
## Principe fondamental
Les appareils Meshtastic transmettent toutes les données en **unités SI métriques**. L'application enveloppe les valeurs brutes dans les types Swift `Mesurement` avec l'unité source correcte, puis permet au système d'exploitation de les formater pour les paramètres régionaux de l'utilisateur. Aucune ramification manuelle `if métrique ... sinon impériale` n'est nécessaire pour la plupart des quantités.
```
Device (protobuf, always metric)
  → Wrap in Measurement<Unit>(value:, unit: .sourceUnit)
  → Format with .formatted(.measurement(...)) or MeasurementFormatter
  → Display (auto-converted to user's locale)
```

## Unités sources Protobuf
Ce sont les unités canoniques que l'appareil envoie. Utilisez-les toujours comme unité source lors de la construction des valeurs de mesure :
| Quantité | Unité d'appareil | Type d'unité Swift |
|----------|------------|-----------------|
| Altitude | Mètres | `Unité de longueur.mètres` |
| Distance (capteur) | Millimètres | `UnitéLongueur.millimètres` |
| Vitesse au sol | Km/h | `Vitesse unitaire.kilomètres par heure` |
| Vitesse du vent / Rafale | M/s | `Vitesse de l'unité.mètres par seconde` |
| Température | °C | `UnitéTempérature.celsius` |
| Pression barométrique | HPa | `UnitéPressure.hectopascals` |
| Précipité (1h / 24h) | Mm | `UnitéLongueur.millimètres` |
| Poids | Kg | `UnitéMass.kilogrammes` |

> **Avertissement - `CLLocation.speed` renvoie m/s, pas km/h. ** Lorsque vous enveloppez la vitesse GPS, utilisez `UnitSpeed.metersPerSecond`. L'erreur de l'unité source produit des conversions silencieusement incorrectes.
## API de formatage
### `.formaté(.mesure(...)) `
Préféré pour le texte en ligne. Convertit automatiquement en paramètres régionaux de l'utilisateur :
```swift
let speed = Measurement(value: newLocation.speed, unit: UnitSpeed.metersPerSecond)
Text(speed.formatted(.measurement(width: .abbreviated,
    numberFormatStyle: .number.precision(.fractionLength(0)))))
// → "12 km/h" or "7 mph"
```

### `Formateur de mesure`
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
Utilisez l'extension `formattedTemperature()` sur `Float` (définie dans `Meshtastic/Extensions/Float.swift`) :
```swift
// Auto-converts °C → °F based on locale
Text(temperature.formattedTemperature())
```

Lorsque vous avez besoin de la valeur convertie brute (par exemple, pour les points de données du graphique), utilisez `localeTemperature()` :
```swift
let displayValue = temperature.localeTemperature()  // Double in user's preferred unit
```

Les deux méthodes utilisent `kCFLocaleTemperatureUnitKey` pour détecter la préférence de température de l'utilisateur.
## Détection des paramètres régionaux
### Unité de température
```swift
let locale = NSLocale.current as NSLocale
let localeUnit = locale.object(forKey: NSLocale.Key(rawValue: "kCFLocaleTemperatureUnitKey"))
if (localeUnit as? String) == "Fahrenheit" {
    // Use .fahrenheit
}
```

> **Avertissement - Ne forcez jamais les requêtes locales.** `localeUnit` peut être `nil` sur certaines versions du système d'exploitation. Utilisez toujours `as ? String` avec un défaut sûr (Celsius).
### Système de mesure
```swift
let usesMetric = Locale.current.measurementSystem == .metric
```

Utilisé pour les quantités où le formatage `Mesurement` ne s'applique pas entièrement (par exemple, choisir la précision décimale pour les précipitations : 0 décimales pour mm, 1 pour les pouces).
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
| Indicateurs de récence | `RelativeDateTimeFormater()` | "Il y a 5 minutes" |
| Horodatages | `Date.formaté(date : .numérique, heure : .raccourci)` | "5/9/26, 14h30" |
| Modèles de paramètres régionaux | `DateFormatter.dateFormat(fromTemplate:options:locale:)` | Respecte 12/24h, ordre de date |
| Exporter (lisible par machine) | `DateFormatter` avec `en_US_POSIX` paramètre | "2026-05-09_143000" |
| TAK/CoT XML | `Date.ISO8601FormatStyle` | ISO 8601 avec fraction de secondes |

Ne codez jamais au format 12 heures ou 24 heures - laissez le système d'exploitation le gérer via des formateurs de paramètres régionaux.
## Carte des fichiers
| Fichier | Ce qu'il fait |
|------|-------------|
| `Extensions/Float.swift` | `Température formatée()`, `Température locale()` |
| `Vues/Paramètres/GPSStatus.swift` | Formatage de la vitesse GPS (source m/s) |
| `Vues/Aides/Météo/Conditions météorologiques locales.swift` | Température et vent de WeatherKit |
| `Vues/Aides/Météo/NodeWeatherForecast.swift` | Conversion de la température des prévisions horaires |
| `Vues/Nods/Aides/Carte/PositionAltitudeChart.swift` | Axe de carte d'altitude conscient de la région |
| `Vues/Nods/Helpers/NodeDetail.swift` | Affichage du poids, des précipitations, du vent, de la température du sol |
| `Vues/Noeuds/Aides/Colonnes de métriques/EnvironnementDefaultColumns.swift` | Colonnes de table de télémétrie |
| `Vues/Noeuds/Aides/Colonnes de métriques/EnvironnementDefaultSeries.swift` | Graphique des seuils de température du gradient |
| `Vues/Aides/DistanceText.swift` | Emballage `MKDistanceFormatter` |
| `Vues/Aides/CompassView.swift` | `MeasurementFormatter` avec `.naturalScale` |
| `Mesure/CustomFormatters.swift` | `altitudeFormatter` partagé |

## Liste de contrôle pour les nouveaux champs de télémétrie
Lors de l'ajout d'une nouvelle valeur de capteur ou d'un écran de télémétrie :
1. Identifier l'unité source protobuf à partir du schéma du dispositif2. Enveloppez dans `Mesurement<Unit>(value:, unit:)` avec l'unité source correcte3. Format avec `.formatted(.measurement(...))` - ne codez pas de chaînes d'unité4. S'il s'agirt d'un graphique, assurez-vous que les étiquettes d'axe utilisent la même conversion de localisation5. S'il s'agit d'une unité universelle (hPa, degrés, %), affichez-la telle quelle6. Testez avec les paramètres du système de mesure américain et métrique dans le simulateur
