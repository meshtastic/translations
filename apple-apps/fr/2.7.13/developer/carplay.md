
# Architecture CarPlay
Cette page couvre la mise en œuvre de la fonctionnalité CarPlay. Pour le guide de l'utilisateur, voir [CarPlay](../user/carplay.md).
## Composants
| Composant | Fichier | Description |
|---|---|---|
| `CarPlaySceneDélégate` | `Meshtastic/CarPlay/CarPlaySceneDelegate.swift` | `CPTemplateApplicationSceneDelegate` qui construit et gère l'interface utilisateur à deux onglets |
| `CarPlayIntentDonation` | `Meshtastic/CarPlay/CarPlayIntentDonation.swift` | Donne des interactions `INSendMessageIntent` entrantes et sortantes afin que les conversations apparaissent dans les messages CarPlay et que Siri puisse les lire à haute voix |
| `EnvoyerMessageIntentHandler` | `Meshtastic/Intents/SendMessageIntentHandler.swift` | Gère `INSendMessageIntent` - résout les destinataires/canaux et envoie le message sur le transport actif |
| `Recherche de messagesIntentHandler` | `Meshtastic/Intentions/Recherche de messagesIntentHandler.swift` | Gère `INSearchForMessagesIntent` |
| `SetMessageAttributeIntentHandler` | `Meshtastic/Intentions/SetMessageAttributeIntentHandler.swift` | Gère `INSetMessageAttributeIntent` (marquer comme lu) |
| `IntentHandler` | `Meshtastic/Intentions/IntentHandler.swift` | Achemine `INintention`s vers le gestionnaire approprié |

## Mises à jour du modèle
Le délégué de la scène souscrit à`AccessoryManager.shared.$isConnected`Avec un **300 ms debounce** et des appels`updateSections(_:)`Sur l'existant`CPListTemplate`Instances plutôt que de reconstruire l'ensemble de l'arborescence des modèles. Cela minimise le scintillement lors des reconnexions et évite de déclencher la limite de vitesse de CarPlay lors du remplacement du modèle.
## Déduplication du don intentionnel
Les dons d'intention sont dédupliqués par session CarPlay à l'aide d'un en mémoire`Set`. Cela évite les appels IPC répétés au démon d'intentions à chaque rafraîchissement de liste (ce qui se produit sur une minuterie lorsque CarPlay est connecté).
Lorsqu'une nouvelle session CarPlay commence, l'ensemble est effacé et jusqu'à 50 messages non lus sont donnés par lots afin que Siri puisse les relire à la demande.
## Ajout d'une nouvelle intention
1. Créer un gestionnaire dans`Meshtastic/Intents/`Conforme à l'approprié`INIntent`Protocole.2. Enregistrer le gestionnaire dans`IntentHandler.swift`'S`handler(for:)`Commutateur.3. Déclarer l'intention dans`Meshtastic.entitlements`Sous`com.apple.developer.siri`.4. Ajouter une description d'utilisation dans`Info.plist`Si l'intention nécessite une nouvelle autorisation de confidentialité.
