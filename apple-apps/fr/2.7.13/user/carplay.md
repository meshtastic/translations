
# CarPlay
L'application prend en charge Apple CarPlay pour la **messagerie maillé mains libres** pendant la conduite. L'interface CarPlay s'intègre au système de messagerie iOS et à Siri afin que les utilisateurs puissent envoyer et recevoir des messages Meshtastic sans regarder leur téléphone.
## Exigences
- IPhone exécutant iOS 18 ou version ultérieure- Une unité principale CarPlay prise en charge ou le simulateur CarPlay dans Xcode- Un appareil Meshtastic connecté via Bluetooth, TCP ou série- Siri activé - l'application demande l'autorisation de Siri pendant l'intégration et à nouveau lors des lancements ultérieurs
## Interface
L'écran CarPlay présente une **interface à deux onglets** :
| Onglet | Description |
|-----|-------------|
| **Canaux** | Répertorie tous les canaux de maillage actifs |
| **Messages directs** | Répertorie les contacts récents et préférés |

Lorsqu'aucun appareil Meshtastic n'est connecté, les deux onglets affichent un élément d'état **"Non connecté"** avec une invite à l'ouverture de l'application Meshtastic.
### Onglet Chaînes
Chaque ligne de canal montre :- Le nom de la chaîne (ou "Chaîne principale" pour l'index 0)- Un badge de message non lu lorsqu'il y a des messages non lus- "Primaire" ou "Ch N" comme texte détaillé
Le fait de taper sur une ligne de chaîne lance une session de composition Siri pour cette chaîne.
### Onglet Messages directs
L'onglet Messages directs est divisé en deux sections :
- **Favoris** - Nouds marqués comme favoris, triés par derniers entendus- **Récent** - Tous les autres contacts messageables avec l'historique, triés par dernière audience (plas de 24 entrées)
Chaque ligne de contact montre :- Nom du contact et une icône de personne- Nombre de messages non lus le cas échéant- Temps écoulé depuis la dernière fois entendu (par exemple, "Juste maintenant", "il y a 5m", "il y a 2h", "il y a 3j")
## Commandes vocales Siri
Utilisez ces commandes vocales Siri dans CarPlay pour interagir avec Meshtastic :
| Commande vocale | Exemple de phrase | Description |
|---|---|---|
| Envoyer un message | "Envoyer un message sur Meshtastic" | Compose et envoie un message texte à un contact ou à un canal |
| Rechercher des messages | "Rechercher des messages Meshtastic" | Recherche l'historique des messages |
| Marquer comme lu | "Marquer le message Meshtastic comme lu" | Marque une conversation comme lue |

> **Avertissement - Limites de message :**> Les messages sont limités à **200 octets** (UTF-8). Siri n'enverra pas de messages dépassant cette limite. Seul un destinataire** par message est pris en charge - pas de messages directs de groupe. Les messages Emoji uniquement et les messages d'administrateur sont exclus de CarPlay.
## Annonces de messages entrants
Lorsque CarPlay est connecté et que **Annoncer les notifications** est activé dans Paramètres iOS → Siri, Siri lit à haute voix les messages Meshtastic entrants. Seuls les messages texte non-emoji et non administrateurs déclenchent des annonces.
Jusqu'à 50 messages non lus qui sont arrivés avant le début de la session CarPlay sont donnés à Siri au moment de la connexion afin qu'ils puissent être lus à la demande.
## Activité en direct
Lorsqu'un appareil Meshtastic se connecte pendant une session CarPlay, une **Dynamic Island / Lock Screen Live Activity** démarre automatiquement (iOS uniquement, non disponible sur macOS). Il affiche :
- Nom du nœud et nom court- Temps de disponibilité, utilisation du canal et pourcentage de temps d'antenne TX- Paquets envoyés, reçus et statistiques de relais- Nombre de nœuds en ligne et total- Un compte à rebours de 15 minutes synchronisé avec l'intervalle de rapport de télémétrie
L'activité en direct se termine automatiquement lorsque CarPlay se déconnecte.
> **Astuce —** Pour les détails de la mise en œuvre et l'architecture des composants, consultez le [Guide du développeur CarPlay](../developer/carplay.md).

