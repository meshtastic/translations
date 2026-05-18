
# Unités, mesure et paramètres régionaux

L'application Meshtastic affiche automatiquement les températures, les distances, les vitesses et les temps dans les unités que votre appareil est configuré pour utiliser - aucun paramètre à modifier dans l'application.

---

## Comment ça marche

Les radios Meshtastic transmettent toujours des données en **unités métriques** (mètres, °C, km/h, hPa, etc.). Lorsque l'application reçoit ces données, elle les remet au système de formatage intégré de votre appareil, qui convertit et affiche les valeurs dans n'importe quel système unitaire que vous avez choisi dans **Paramètres → Général → Langue et région**.

![Language & Region settings](../assets/screenshots/settingsLanguageRegion.png)

L'écran **Langue et région** contrôle la façon dont l'application Meshtastic affiche les températures, les distances, les dates, les chiffres et plus encore. Paramètres clés :

| Cadre | Ce qu'il contrôle dans Meshtastic |
|---|---|
| **Température** | °C ou °F pour toutes les lectures des capteurs et la météo |
| **Système de mesure** | Métrique (m, km, kg, mm) ou États-Unis/Royaume-Uni (ft, mi, lbs, in) |
| **Calendrier** | Système de calendrier pour toutes les dates |
| **Premier jour de la semaine** | Le jour du début de la semaine dans les affichages de la date |
| **Format de date** | Date de commande dans toute l'application |
| **Format du numéro** | Séparateurs décimaux et regroupement de chiffres |

> **Tip — Pas besoin de changer d'unité**
> Modifiez vos préférences de mesure du système et chaque écran de Meshtastic se met à jour automatiquement - détails sur les nœuds, graphiques de télémétrie, météo, altitude, et plus encore.

## Température

Les valeurs de température des capteurs environnementaux et des prévisions météorologiques sont transmises en **°C** et affichées en **°C** ou **°F** en fonction de la préférence de l'unité de température de votre appareil.

| Votre réglage | Tu vois |
|---|---|
| Celsius | 22 °C |
| Fahrenheit | 72 °F |

Cela affecte tous les affichages de température dans toute l'application : télémétrie de l'environnement des nœuds, température du sol, point de rosée, prévisions météorologiques et axes graphiques de télémétrie.

## Distance et altitude

Les distances entre les nœuds et les altitudes GPS sont transmises en **mètres** et automatiquement mises à l'échelle et converties par le système.

| Votre réglage | Petite distance | Grande distance | Altitude |
|---|---|---|---|
| Métrique | 350 mètres | 2,5 kilomètres | 1 200 m |
| Impérial (États-Unis) | 1 148 pieds | 1,6 mi | 3 937 pieds |

L'application utilise la mise à l'échelle naturelle - les courtes distances restent en mètres ou en pieds, tandis que les longues distances passent automatiquement en kilomètres ou en miles.

### Où ceux-ci apparaissent

- **Liste des nœuds** - distance et incidence par rapport à chaque nœud
- **Détail du nœud** - altitude, distance de votre position
- **Carte** - distances de waypoint, distances de saut d'itinéraire de trace
- **Compas** - distance par rapport au nœud sélectionné
- **Graphique d'altitude** - Les étiquettes de l'axe Y s'adaptent à votre paramètre régional

## Vitesse

La vitesse au sol GPS est affichée dans l'unité de vitesse préférée de votre région.

| Votre réglage | Tu vois |
|---|---|
| Métrique | 12 km/h |
| Impérial (États-Unis) | 7 mph |

La vitesse apparaît sur l'écran **Statut GPS** lorsque votre appareil dispose d'un correctif GPS actif.

## Vent

Les données sur la vitesse du vent et les rafales des capteurs d'environnement sont transmises en **m/s** et converties pour l'affichage.

| Votre réglage | Tu vois |
|---|---|
| Métrique | 5 m/s |
| Impérial (États-Unis) | 11 mph |

Les lectures du vent apparaissent dans la section météo **Détail du nœud** et les colonnes du journal **Telemetry de l'environnement**.

## Poids

La télémétrie de poids est transmise en **kg** et convertie pour l'affichage.

| Votre réglage | Tu vois |
|---|---|
| Métrique | 24,5 kg |
| Impérial (États-Unis) | 54,0 livres |

## Niveau de précipitations

Les mesures des précipitations (total de 1 heure et 24 heures) sont transmises en **mm** et converties pour l'affichage.

| Votre réglage | Tu vois |
|---|---|
| Métrique | 12 mm |
| Impérial (États-Unis) | 0,5 in |

## Des unités qui ne changent jamais

Certaines unités sont des normes internationales et sont affichées de la même manière, quelle que soit votre région :

| Dimension | Unité | Pourquoi |
|---|---|---|
| Pression barométrique | HPa | Norme météorologique internationale |
| Direction / roulement | ° (degrés) | Convention de navigation universelle |
| Rayonnement | µR/h | Unité de dosimétrie standard |
| Coordonnées GPS | Degrés décimaux | Norme géographique universelle |
| Humidité, batterie, humidité du sol | % | Universel |

## Date et heure

Tous les horodatages de l'application - dernière écoute, heures de message, journaux de télémétrie, axes graphiques - suivent les préférences de date et d'heure de votre appareil.

| Cadre | Ce qu'il contrôle | Exemple |
|---|---|---|
| **Temps de 24 heures** | Format d'horloge | 14h30 contre 14h30 |
| **Format de date** | Date de commande | 09/05/2026 vs 05/09/2026 vs 2026-05-09 |
| **Calendrier** | Système de calendrier | Grégorien, bouddhiste, japonais, etc. |

L'application utilise également **temps relatif** là où cela a du sens - par exemple, "il y a 5 minutes" ou "il y a 2 heures" dans la liste des nœuds - qui est automatiquement localisé dans la langue de votre appareil.

## Modification de votre système de mesure

Votre système de mesure (métrique vs impérial) est lié à votre paramètre régional. Pour le changer sans changer votre langue :

1. Ouvrir **Paramètres → Général → Langue et région**
2. Appuyez sur **Système de mesure**
3. Choisissez **Métrique**, **États-Unis** ou **Royaume-Uni**

L'application Meshtastic reprend le changement immédiatement - aucun redémarrage n'est nécessaire.

> **Tip — Royaume-Uni contre Impérial américain**
> Le système de mesure britannique utilise des miles pour la distance, mais des pierres pour le poids corporel et Celsius pour la température. Le système américain utilise Fahrenheit et livres. L'application respecte automatiquement ces distinctions.

