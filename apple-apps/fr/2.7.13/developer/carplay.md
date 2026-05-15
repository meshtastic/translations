
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
Le délégué de scène s'abonne à `AccessoryManager.shared.$isConnected` avec un **300 ms debounce** et appelle `updateSections(_:)` sur les instances `CPListTemplate` existantes plutôt que de reconstruire l'ensemble de l'arborescence des modèles. Cela minimise le scintillement lors des reconnexions et évite de déclencher la limite de vitesse de CarPlay lors du remplacement du modèle.
## Intention de faire un don Dé-duplication
Les dons d'intention sont dédupliqués par session CarPlay à l'aide d'un `Set` en mémoire. Cela évite les appels IPC répétés au démon d'intentions à chaque rafraîchissement de liste (ce qui se produit sur une minuterie lorsque CarPlay est connecté).
Lorsqu'une nouvelle session CarPlay commence, l'ensemble est effacé et jusqu'à 50 messages non lus sont donnés par lots afin que Siri puisse les relire à la demande.
## Ajout d'une nouvelle intention
1. Créez un gestionnaire dans `Meshtastic/Intents/` conforme au protocole `INIntent` approprié.2. Enregistrez le gestionnaire dans le commutateur `handler(for:)` de `IntentHandler.swift`.3. Déclarez l'intention dans `Meshtastic.entitlements` sous `com.apple.developer.siri`.4. Ajoutez une description d'utilisation dans `Info.plist` si l'intention nécessite une nouvelle autorisation de confidentialité.
