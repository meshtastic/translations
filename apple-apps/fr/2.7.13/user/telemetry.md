
# Télémétrie et capteurs
Les nœuds Meshtastic peuvent rapporter les données des capteurs à travers le maillage, ce qui vous donne une visibilité sur l'environnement physique à distance.
## Types de télémétrie
| Type | Données |
|------|------|
| Mesures de l'appareil | Niveau de la batterie, tension de la batterie, utilisation des canaux, fraction de temps d'antenne |
| Environnement | Température (°C/°F), humidité relative (%), pression barométrique (hPa) |
| Qualité de l'air | Nombre de particules PM1.0, PM2.5, PM10 (µg/m³) |
| Puissance | Lectures de tension et de courant à partir des capteurs de surveillance de la puissance |

### Mesures de l'appareil
| Icône | État | Description |
|------|-------|-------------|
| ![Battery full](../assets/screenshots/batteryFull.png) | Plein | La batterie est bien chargée (≥80 %). |
| ![Battery low](../assets/screenshots/batteryLow.png) | Bas | La batterie est faible (≤20 %) - chargez rapidement le nœud. |
| ![Battery charging](../assets/screenshots/batteryCharging.png) | Chargement | Le nœud est branché et complètement chargé. |
| ![Battery unknown](../assets/screenshots/batteryNil.png) | Inconnu | Niveau de la batterie non signalé par ce nœud. |
| ![Battery plugged in](../assets/screenshots/batteryPluggedIn.png) | Branché | Le nœud est alimenté par USB/alimentation externe. |

### Qualité de l'air
![IAQ Scale](../assets/screenshots/iaqScale.png)

L'échelle de qualité de l'air intérieur montre les bandes de catégorie d'Excellent (vert) à Dangereux (marron). L'application prend en charge plusieurs modes d'affichage pour les lectures de la qualité de l'air :
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/aqi_all_modes_dark.png" />
  <img src="../assets/screenshots/aqi_all_modes_light.png" alt="Air Quality Index — all display modes" />
</picture>

### Environnement
| Icône | Lecture | Description |
|------|---------|-------------|
| ![Humidity with dew point](../assets/screenshots/humidityWithDew.png) | Humidité (avec point de rosée) | Pourcentage d'humidité relative et température du point de rosée calculée. |
| ![Humidity without dew point](../assets/screenshots/humidityNoDew.png) | Humidité | Pourcentage d'humidité relative uniquement. |
| ![Pressure high](../assets/screenshots/pressureHigh.png) | À haute pression | Pression barométrique supérieure à la normale (≥1013 hPa). |
| ![Pressure low](../assets/screenshots/pressureLow.png) | Basse pression | Pression barométrique inférieure à la normale (<1013 hPa). |

### Vent
| Widget | Description |
|--------|-------------|
| ![Wind full](../assets/screenshots/windFull.png) | Vitesse du vent, vitesse de la rafale et direction. |
| ![Wind minimal](../assets/screenshots/windMinimal.png) | Vitesse du vent uniquement (pas de données de rafale ou de direction disponibles). |

### Rayonnement
| Widget | Description |
|--------|-------------|
| ![Radiation](../assets/screenshots/radiation.png) | Niveau de rayonnement en µR/h à partir d'un capteur de compteur Geiger connecté. |

## Visualisation de la télémétrie
La télémétrie est visible à deux endroits :
1. **Détail du nœud** - appuyez sur n'importe quel nœud dans l'onglet Nodes. La section Journaux affiche les métriques les plus récentes de l'appareil et les lectures d'environnement.2. **Graphiques de télémétrie** - appuyez sur l'icône du graphique dans un détail de nœud pour voir les graphiques historiques de tout type de télémétrie que le nœud a signalé.
## Configuration de la télémétrie
Allez dans **Paramètres → Télémétrie** pour activer les modules de télémétrie et définir des intervalles de rapport :
![Telemetry Config](../assets/screenshots/telemetryConfig.png)

| Cadre | Description |
|---------|-------------|
| Intervalle des métriques de l'appareil | À quelle fréquence (secondes) le nœud diffuse les données de batterie et d'utilisation. |
| Intervalle d'environnement | À quelle fréquence les données des capteurs d'environnement sont-elles diffusées ? |
| Intervalle de qualité de l'air | À quelle fréquence les données des capteurs de qualité de l'air sont-elles diffusées ? |
| Écran d'environnement | Afficher les données d'environnement sur l'écran de l'appareil. |
| Télémétrie sur le canal d'administration | Limiter la télémétrie au canal d'administration au lieu de la diffusion. |

## Capteurs pris en charge
L'application affiche les données de n'importe quel capteur pris en charge par le micrologiciel Meshtastic. Capteurs communs :
- **BME280 / BME680** — température, humidité, pression- **SHT31** - température, humidité- **MCP9808** - température de précision- **INA219 / INA260** - surveillance de l'alimentation- **PMSA003** — qualité de l'air (PM2.5)
La disponibilité du capteur dépend de votre matériel. Consultez le [guide matériel Meshtastic](https://meshtastic.org/docs/hardware/) pour la compatibilité.
## Capteur de détection
Le module de capteur de détection alerte le maillage lorsqu'un capteur de mouvement PIR connecté ou un commutateur de contact est déclenché. Configurez-le dans **Paramètres → Capteur de détection**. Les alertes apparaissent sous forme de messages sur le canal principal et sous forme d'entrées de journal de nœud.
