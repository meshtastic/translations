
# MQTT

Le module MQTT permet à un nœud Meshtastic de relier le trafic maillé à un courtier MQTT, étendant le maillage sur Internet et permettant l'intégration avec les systèmes domotiques.

## Ce que fait MQTT

Un nœud avec MQTT activé agit comme une passerelle : il publie des paquets de maillage reçus à un courtier MQTT et s'abonne éventuellement à un sujet afin que les nœuds distants puissent injecter des paquets dans le maillage local.

| Icône | État | Description |
|------|-------|-------------|
| ![MQTT connected](../assets/screenshots/mqttConnected.png) | Connecté | Le pont MQTT est actif - liaison montante et descendante toutes deux activées. |
| ![MQTT uplink only](../assets/screenshots/mqttUplinkOnly.png) | Liaison montante uniquement | Publier des paquets de maillage au courtier, mais ne pas s'abonner aux paquets entrants. |
| ![MQTT disconnected](../assets/screenshots/mqttDisconnected.png) | Décousu | MQTT est configuré mais n'est pas actuellement connecté au courtier. |

Cela permet à deux réseaux maillés dans des emplacements physiques différents d'apparaître comme un réseau logique - tant qu'au moins un nœud dans chaque emplacement a un accès à Internet.

## Configuration de MQTT

Allez dans **Paramètres → MQTT** :

![MQTT Config](../assets/screenshots/mqttConfig.png)

| Cadre | Description |
|---------|-------------|
| Serveur MQTT | Nom d'hôte ou adresse IP de votre courtier MQTT (par exemple, `mqtt.meshtastic.org` pour le courtier public). |
| Port | La valeur par défaut est 1883 (non cryptée) ou 8883 (TLS). |
| Nom d'utilisateur | Nom d'utilisateur du courtier MQTT (facultatif). |
| Mot de passe | Mot de passe du courtier MQTT (facultatif). |
| Sujet racine | Le préfixe de sujet pour tous les messages publiés (par défaut : `msh`). |
| Activé | Activer/désactiver le pont MQTT. |
| Cryptage activé | Cryptez les paquets avant de les publier. Recommandé - empêche le courtier de lire le contenu du message. |
| JSON activé | Publier des paquets JSON décodés en plus du format binaire protobuf. Utile pour les intégrations domotiques. |
| TLS activé | Utilisez TLS pour la connexion MQTT. Nécessite un courtier avec prise en charge TLS. |
| Proxy au client | Acheminez le trafic MQTT via l'application téléphonique plutôt que directement depuis la radio. Utile pour les radios sans Wi-Fi. |

## Structure du sujet

Meshtastic publie à :

```
<root_topic>/<region>/<channel_index>/<node_id>/<packet_type>
```

Exemple :`msh/US/2/!a1b2c3d4/text`

## Considérations de sécurité

- Activation de MQTT avec un emplacement de diffusion de canal non sécurisé et des messages sur Internet.
- L'indicateur de sécurité du canal affiche **Insecure avec MQTT** (🔓⚠️) lorsqu'un canal n'est pas crypté et que MQTT est actif.
- Utilisez toujours **Cryptage activé** en production pour protéger le contenu du message.
- Envisagez d'utiliser un courtier privé plutôt que le public`mqtt.meshtastic.org`.

## Courtier public

Le courtier public MQTT à`mqtt.meshtastic.org`Est disponible pour le test. ** Ne transmettez pas d'informations sensibles sur le courtier public. ** Utilisez-le uniquement pour la vérification initiale de la configuration.

