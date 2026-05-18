
# Liens profonds

L'application enregistre le`meshtastic:///`Schéma d'URL. Utiliser`Router.route(url:)`Pour gérer les URL entrantes. Les liens profonds fonctionnent à partir de Safari, des raccourcis, des intentions Siri et d'autres applications.

## Ajout d'un nouveau lien profond

1. Ajouter un cas à l'approprié`*NavigationState`Énumé dans`Meshtastic/Router/NavigationState.swift`.
2. Mise à jour`Router`Les assistants de routage dans`Meshtastic/Router/Router.swift`.
3. Documentez l'URL dans le tableau ci-dessous.

## Messages

| URL | Description |
|-----|-------------|
| [`meshtastic:///messages`](Meshtastic:///messages) | Onglet Messages |
| `meshtastic:///messages?channelId={channelId}&messageId={messageId}` | Messages de canal (`messageId`Est facultatif) |
| `meshtastic:///messages?userNum={userNum}&messageId={messageId}` | Messages directs (`messageId`Est facultatif) |

## Assurer la correspondance

| URL | Description |
|-----|-------------|
| [`meshtastic:///connect`](Meshtastic:///connect) | Onglet de connexion |

## Noeuds

| URL | Description |
|-----|-------------|
| [`meshtastic:///nodes`](Meshtastic:///nodes) | Onglet Noeuds |
| `meshtastic:///nodes?nodenum={nodenum}` | Nod sélectionné |

## Carte de maillage

| URL | Description |
|-----|-------------|
| [`meshtastic:///map`](Méshtastique:///carte) | Onglet Carte |
| `meshtastic:///map?nodenum={nodenum}` | Noeud sur la carte |
| `meshtastic:///map?waypointId={waypointId}` | Waypoint sur la carte |

## Paramètres

Aucun paramètre n'est pris en charge pour les URL de paramètres.

| URL | Description |
|-----|-------------|
| [`meshtastic:///settings/about`](Meshtastic:///settings/about) | À propos de Meshtastic |
| [`meshtastic:///settings/appSettings`](meshtastic:///settings/appSettings) | Paramètres de l'application |
| [`meshtastic:///settings/helpDocs`](meshtastic:///paramètres/helpDocs) | Aide et documentation |
| [`meshtastic:///settings/routes`](Meshtastic:///settings/routes) | Itinéraires |
| [`meshtastic:///settings/routeRecorder`](meshtastic:///settings/routeRecorder) | Enregistreur d'itinéraire |
| **Configuration de la radio** |  |
| [`meshtastic:///settings/lora`](Meshtastic:///settings/lora) | Configuration LoRa |
| [`meshtastic:///settings/channels`](Meshtastic:///settings/channels) | Canaux |
| [`meshtastic:///settings/security`](Meshtastic:///paramètres/sécurité) | Configuration de sécurité |
| [`meshtastic:///settings/shareQRCode`](meshtastic:///settings/shareQRCode) | Partager le code QR |
| **Configuration de l'appareil** |  |
| [`meshtastic:///settings/user`](Meshtastic:///paramètres/utilisateur) | Configuration de l'utilisateur |
| [`meshtastic:///settings/bluetooth`](Meshtastic:///paramètres/bluetooth) | Configuration Bluetooth |
| [`meshtastic:///settings/device`](Meshtastic:///paramètres/appareil) | Configuration de l'appareil |
| [`meshtastic:///settings/display`](Meshtastic:///settings/display) | Configuration d'affichage |
| [`meshtastic:///settings/network`](Meshtastic:///paramètres/réseau) | Configuration du réseau |
| [`meshtastic:///settings/position`](Meshtastic:///settings/position) | Configuration de la position |
| [`meshtastic:///settings/power`](Meshtastic:///settings/power) | Configuration de l'alimentation |
| **Configuration du module** |  |
| [`meshtastic:///settings/ambientLighting`](méshtastique:///settings/ambientLighting) | Éclairage ambiant |
| [`meshtastic:///settings/cannedMessages`](meshtastic:///settings/cannedMessages) | Messages En Conserve |
| [`meshtastic:///settings/detectionSensor`](meshtastic:///paramètres/détectionSensor) | Capteur de détection |
| [`meshtastic:///settings/externalNotification`](meshtastic:///settings/externalNotification) | Notification externe |
| [`meshtastic:///settings/mqtt`](Meshtastic:///settings/mqtt) | MQTT |
| [`meshtastic:///settings/paxCounter`](meshtastic:///settings/paxCounter) | Comptoir Pax |
| [`meshtastic:///settings/rangeTest`](meshtastic:///settings/rangeTest) | Test de portée |
| [`meshtastic:///settings/ringtone`](Meshtastic:///settings/ringtone) | Sonnerie |
| [`meshtastic:///settings/serial`](Meshtastic:///settings/serial) | Série |
| [`meshtastic:///settings/storeAndForward`](meshtastic:///settings/storeAndForward) | Stocker et transférer |
| [`meshtastic:///settings/telemetry`](Meshtastic:///settings/telemetry) | Télémétrie |
| **PRENEZ** |  |
| [`meshtastic:///settings/tak`](Meshtastic:///settings/tak) | Configuration TAK |
| **Logging** |  |
| [`meshtastic:///settings/debugLogs`](meshtastic:///settings/debugLogs) | Journaux de débogage |
| **Développeurs** |  |
| [`meshtastic:///settings/appFiles`](meshtastic:///settings/appFiles) | Fichiers d'application |
| [`meshtastic:///settings/tools`](Meshtastic:///settings/tools) | Outils (iOS 18+) |
| [`meshtastic:///settings/coreDataBrowser`](meshtastic:///settings/coreDataBrowser) | Navigateur de données (DEBUG uniquement) |
| [`meshtastic:///settings/firmwareUpdates`](meshtastic:///settings/firmwareUpdates) | Mises à jour du micrologiciel |

