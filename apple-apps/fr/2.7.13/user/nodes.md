
# Liste des nœuds
L'onglet Nods affiche tous les appareils que votre radio a entendus sur le maillage. Appuyez sur n'importe quel nœud pour plus de détails.
## Statut du nœud
| Élément | Sens |
|---------|---------|
| ![Node circle](../assets/screenshots/circleTextDefault.png) | **Nom court et nom long** - chaque nœud a un nom court (jusqu'à 4 octets) affiché dans le cercle coloré et un nom long affiché à côté. La couleur du cercle est dérivée du numéro de nœud. Le nom court peut être un emoji ou des initiales. |
| ![Online](../assets/screenshots/nodeOnline.png) | **En ligne** - le nœud a été entendu récemment et est considéré comme en ligne. |
| ![Idle / Sleeping](../assets/screenshots/nodeIdle.png) | **Inactif / En sommeil** - le nœud n'a pas été entendu récemment et peut être endormi ou hors de portée. |
| ![Hops Away](../assets/screenshots/hopsAway.png) | **Hops Away** - le nombre de nœuds intermédiaires relayant des messages entre vous et ce nœud. Pas de saut signifie communication directe. |

## Cryptage
| Icône | Sens |
|------|---------|
| ![Shared Key](../assets/screenshots/lockOpen.png) | **Clé partagée** - les messages directs utilisent la clé partagée pour la chaîne. |
| ![Public Key Encryption](../assets/screenshots/lockClosed.png) | **Cryptage de la clé publique** - les messages directs utilisent l'infrastructure de la clé publique. Nécessite un firmware 2.5+. |
| ![PKI Mismatch](../assets/screenshots/keySlash.png) | **Échec de la clé publique** - la clé publique ne correspond pas à la clé précédemment enregistrée. Vérifiez que le contact est hors bande. |

## Rôles de l'appareil
Chaque nœud est configuré avec un rôle qui détermine son comportement sur le maillage. Les rôles sont affichés dans la vue détaillée du nœud.
| Icône | Rôle | Description |
|------|------|-------------|
| ![](../assets/screenshots/roleClient.png) | Client | Appareil utilisateur final standard. Envoie et reçoit des messages, partage la position. |
| ![](../assets/screenshots/roleClientMute.png) | Client Muet | Comme le client, mais ne transmet pas les paquets d'autres appareils. Réduit le trafic de maillage près des zones encombrées. |
| ![](../assets/screenshots/roleClientHidden.png) | Client caché | Ne diffuse qu'au besoin pour des économies furtives ou d'énergie. |
| ![](../assets/screenshots/roleClientBase.png) | Clientèle | Nœud sur le toit qui distribue largement les messages à partir des nœuds Client Mute à proximité. |
| ![](../assets/screenshots/roleRouter.png) | Router | Noeud d'infrastructure dédié - donne la priorité au transfert de paquets. Pas pour les toits ou les nœuds mobiles. |
| ![](../assets/screenshots/roleRouterLate.png) | Routeur En Retard | Comme le routeur, mais rediffusion une fois après tous les autres nœuds. Mieux adapté aux déploiements sur les toits. |
| ![](../assets/screenshots/roleTracker.png) | Traqueur | Diffuse les paquets de position GPS en priorité. Optimisé pour les rapports de localisation fréquents. |
| ![](../assets/screenshots/roleSensor.png) | Détecteur | Diffuse des paquets de télémétrie en priorité. Optimisé pour les données des capteurs. |
| ![](../assets/screenshots/roleTak.png) | TAK | Optimisé pour la communication du système ATAK. Réduit les émissions de routine. |
| ![](../assets/screenshots/roleTakTracker.png) | Traqueur TAK | Active les émissions automatiques de TAK PLI. Réduit les émissions de routine. |
| ![](../assets/screenshots/roleLostAndFound.png) | Objets trouvés | Diffuse l'emplacement en tant que message au canal par défaut pour faciliter la récupération de l'appareil. |

[Choisir le bon rôle d'appareil →](https://meshtastic.org/blog/choosing-the-right-device-role/)
## Exemples complets de lignes de nœuds
La rangée complète des nœuds affiche l'avatar du cercle, le niveau de la batterie, l'état du cryptage, l'heure de la dernière écoute, le rôle de l'appareil, la force du signal et les indicateurs de journal en même temps.
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/standard_directConnected_dark.png" />
  <img src="../assets/screenshots/standard_directConnected.png" alt="Directly connected node, favorite, with signal meter" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/standard_multiHop_dark.png" />
  <img src="../assets/screenshots/standard_multiHop.png" alt="Multi-hop node 4 hops away" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/standard_mqtt_dark.png" />
  <img src="../assets/screenshots/standard_mqtt.png" alt="MQTT-bridged node" />
</picture>

## Exemples de lignes de nœuds compacts
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_directConnected_allInfo_dark.png" />
  <img src="../assets/screenshots/compact_directConnected_allInfo.png" alt="Directly connected node with all telemetry info" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_multiHop_dark.png" />
  <img src="../assets/screenshots/compact_multiHop.png" alt="Multi-hop node 7 hops away" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_withPosition_dark.png" />
  <img src="../assets/screenshots/compact_withPosition.png" alt="Node with position, 1 hop" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_pkiMismatch_dark.png" />
  <img src="../assets/screenshots/compact_pkiMismatch.png" alt="PKI key mismatch node" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_mqtt_dark.png" />
  <img src="../assets/screenshots/compact_mqtt.png" alt="MQTT-bridged node" />
</picture>

## Icônes supplémentaires
Appuyez sur un nœud et faites défiler jusqu'à la section Journaux pour obtenir des mesures détaillées :
| Registre | Description |
|-----|-------------|
| ![Distance & Bearing](../assets/screenshots/logDistance.png) | Direction et distance jusqu'au nœud en fonction du GPS. Nécessite que les deux appareils partagent la position. |
| ![Channel badge](../assets/screenshots/channelBadge.png) | Le cercle numéroté montre quel canal le nœud utilise. Uniquement montré pour les canaux secondaires (pas le canal principal 0). |
| ![Device Metrics](../assets/screenshots/logDeviceMetrics.png) | Niveau de la batterie, tension, utilisation des canaux et temps d'antenne signalés par le nœud. |
| ![Positions](../assets/screenshots/logPositions.png) | Données de position GPS, y compris la latitude, la longitude et l'altitude. |
| ![Environment](../assets/screenshots/logEnvironment.png) | Données du capteur : température, humidité, pression barométrique. |
| ![Detection Sensor](../assets/screenshots/logDetectionSensor.png) | Alertes de mouvement ou d'ouverture/fermeture de porte du nœud. |
| ![Trace Routes](../assets/screenshots/logTraceRoutes.png) | Chemins de traçage enregistrés montrant les sauts qu'un message a pris à travers le maillage. |

## Vue détaillée du nœud
Appuyez sur n'importe quel nœud pour voir la vue détaillée complète avec les informations matérielles, les mesures de signal, les capteurs d'environnement et la navigation du journal :
![Node Detail](../assets/screenshots/nodeDetail.png)

[Documents de configuration de l'appareil →](https://meshtastic.org/docs/configuration/radio/device/)
