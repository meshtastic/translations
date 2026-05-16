
# Mises à jour du micrologiciel

L'application peut vérifier et installer les mises à jour du micrologiciel Meshtastic directement sur votre radio connectée via Bluetooth.

## Vérification des mises à jour

1. Connectez-vous à votre radio.
2. Allez dans **Paramètres → Mises à jour du micrologiciel**.
3. L'application affiche la version du firmware actuellement en cours d'exécution sur votre radio et la dernière version stable disponible sur GitHub.

## Installation d'une mise à jour

1. Appuyez sur **Mettre à jour le micrologiciel** lorsqu'une version plus récente est disponible.
2. L'application télécharge le bon binaire du micrologiciel pour votre matériel.
3. La radio entre en mode de mise à jour (DFU) et le nouveau firmware est transféré sur BLE.
4. La radio redémarre automatiquement lorsque la mise à jour est terminée.

| Icône | Progrès | Description |
|------|----------|-------------|
| ![0%](../assets/screenshots/progressZero.png) | Début | Mise à jour du démarrage - téléchargement binaire du micrologiciel. |
| ![50%](../assets/screenshots/progressHalf.png) | En cours | Transfert du micrologiciel en cours sur BLE. |
| ![Complete](../assets/screenshots/progressComplete.png) | Complet | Transfert terminé - la radio redémarre. |
| ![Error](../assets/screenshots/progressError.png) | Erreur | La mise à jour a échoué - voir Dépannage ci-dessous. |

**Pas fermer l'application et ne pas sortir de la portée Bluetooth pendant une mise à jour du micrologiciel. **

## Mettre à jour les canaux

| Canal | Description |
|---------|-------------|
| Stable | Recommandé pour la plupart des utilisateurs. Versions testées. |
| Alpha | Accès anticipé - peut contenir des bugs. Utiliser uniquement sur des appareils secondaires/de test. |

Sélectionnez le canal de mise à jour dans **Paramètres → Mises à jour du micrologiciel**.

## Diagnostic des anomalies

**Échec de la mise à jour à mi-chemin**
- Gardez la radio à un rayon de 1 à 2 mètres de votre téléphone pendant la mise à jour.
- Si la radio semble brickée après un échec de mise à jour, elle peut généralement être récupérée à l'aide du [Meshtastic Flasher](https://flasher.meshtastic.org/) sur un ordinateur.

![Incompatible firmware version warning](../assets/screenshots/invalidVersion.png)

![Security update recommended](../assets/screenshots/securityVersionNag.png)

**Radio n'apparaît pas dans la liste des micrologiciels**
- La fonction de mise à jour du micrologiciel nécessite une radio connectée (BLE ou TCP).
- Certaines radios plus anciennes ne prennent pas en charge les mises à jour OTA. Consultez la [liste de compatibilité matérielle](https://meshtastic.org/docs/hardware/).

**Version affichée comme inconnue**
- Assurez-vous que la radio est entièrement connectée et synchronisée (il faut généralement 5 à 10 secondes après l'établissement de la connexion BLE).

