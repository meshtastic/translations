
# Liens profonds
L'application enregistre le`meshtastic:///`Schéma d'URL. Utiliser`Router.route(url:)`Pour gérer les URL entrantes. Les liens profonds fonctionnent à partir de Safari, des raccourcis, des intentions Siri et d'autres applications.
## Ajout d'un nouveau lien profond
1. Ajouter un cas à l'approprié`*NavigationState`Énumé dans`Meshtastic/Router/NavigationState.swift`.2. Mise à jour`Router`Les assistants de routage dans`Meshtastic/Router/Router.swift`.3. Documentez l'URL dans le tableau ci-dessous.
## Messages
| URL | Description |
|-----|-------------|
| [`meshtastic:///messages`](meshtastic:///messages) | Onglet Messages |
| `meshtastic:///messages ? channelId={channelID}&messageId={messageID}` | Messages de canal (`messageId` est facultatif) |
| `meshtastic:///messages ? Numéro d'utilisateur={Numéro d'utilisateur}&MessageId={MessageId}` | Messages directs (`messageId` est facultatif) |

## Se connecter
| URL | Description |
|-----|-------------|
| [`meshtastic:///connect`](meshtastic:///connect) | Onglet de connexion |

## Noeuds
| URL | Description |
|-----|-------------|
| [`meshtastic:///nodes`](meshtastic:///nodes) | Onglet Noeuds |
| `meshtastic:///nodes ? Nodenum={nodenum}` | Nod sélectionné |

## Carte de maillage
| URL | Description |
|-----|-------------|
| [`meshtastic:///map`](meshtastic:///map) | Onglet Carte |
| `meshtastic:///map ? Nodenum={nodenum}` | Noeud sur la carte |
| `meshtastic:///map ? waypointId={waypointId}` | Waypoint sur la carte |

## Paramètres
Aucun paramètre n'est pris en charge pour les URL de paramètres.
| URL | Description |
|-----|-------------|
| [`meshtastic:///settings/about`](meshtastic:///settings/about) | À propos de Meshtastic |
| [`meshtastic:///settings/appSettings`](meshtastic:///settings/appSettings) | Paramètres de l'application |
| [`meshtastic:///settings/helpDocs`](meshtastic:///settings/helpDocs) | Aide et documentation |
| [`meshtastic:///settings/routes`](meshtastic:///settings/routes) | Itinéraires |
| [`meshtastic:///settings/routeRecorder`](meshtastic:///settings/routeRecorder) | Enregistreur d'itinéraire |
| **Configuration de la radio** |  |
| [`meshtastic:///settings/lora`](meshtastic:///settings/lora) | Configuration LoRa |
| [`meshtastic:///settings/channels`](meshtastic:///settings/channels) | Canaux |
| [`meshtastic:///settings/security`](meshtastic:///settings/security) | Configuration de sécurité |
| [`meshtastic:///settings/shareQRCode`](meshtastic:///settings/shareQRCode) | Partager le code QR |
| **Configuration de l'appareil** |  |
| [`meshtastic:///settings/user`](meshtastic:///settings/user) | Configuration de l'utilisateur |
| [`meshtastic:///settings/bluetooth`](meshtastic:///settings/bluetooth) | Configuration Bluetooth |
| [`meshtastic:///settings/device`](meshtastic:///settings/device) | Configuration de l'appareil |
| [`meshtastic:///settings/display`](meshtastic:///settings/display) | Configuration d'affichage |
| [`meshtastic:///settings/network`](meshtastic:///settings/network) | Configuration du réseau |
| [`meshtastic:///settings/position`](meshtastic:///settings/position) | Configuration de la position |
| [`meshtastic:///settings/power`](meshtastic:///settings/power) | Configuration de l'alimentation |
| **Configuration du module** |  |
| [`meshtastic:///settings/ambientLighting`](meshtastic:///settings/ambientLighting) | Éclairage ambiant |
| [`meshtastic:///settings/cannedMessages`](meshtastic:///settings/cannedMessages) | Messages En Conserve |
| [`meshtastic:///settings/detectionSensor`](meshtastic:///settings/detectionSensor) | Capteur de détection |
| [`meshtastic:///settings/externalNotification`](meshtastic:///settings/externalNotification) | Notification externe |
| [`meshtastic:///settings/mqtt`](meshtastic:///settings/mqtt) | MQTT |
| [`meshtastic:///settings/paxCounter`](meshtastic:///settings/paxCounter) | Comptoir Pax |
| [`meshtastic:///settings/rangeTest`](meshtastic:///settings/rangeTest) | Test de portée |
| [`meshtastic:///settings/ringtone`](meshtastic:///settings/ringtone) | Sonnerie |
| [`meshtastic:///settings/serial`](meshtastic:///settings/serial) | Série |
| [`meshtastic:///settings/storeAndForward`](meshtastic:///settings/storeAndForward) | Stocker et transférer |
| [`meshtastic:///settings/telemetry`](meshtastic:///settings/telemetry) | Télémétrie |
| **PRENEZ** |  |
| [`meshtastic:///settings/tak`](meshtastic:///settings/tak) | Configuration TAK |
| **Logging** |  |
| [`meshtastic:///settings/debugLogs`](meshtastic:///settings/debugLogs) | Journaux de débogage |
| **Développeurs** |  |
| [`meshtastic:///settings/appFiles`](meshtastic:///settings/appFiles) | Fichiers d'application |
| [`meshtastic:///settings/tools`](meshtastic:///settings/tools) | Outils (iOS 18+) |
| [`meshtastic:///settings/coreDataBrowser`](meshtastic:///settings/coreDataBrowser) | Navigateur de données (DEBUG uniquement) |
| [`meshtastic:///settings/firmwareUpdates`](meshtastic:///settings/firmwareUpdates) | Mises à jour du micrologiciel |

