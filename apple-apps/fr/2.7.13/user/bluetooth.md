
# Connexion de l'appareil Bluetooth

L'application Meshtastic se connecte à votre radio via Bluetooth Low Energy (BLE). Vous pouvez gérer plusieurs radios et basculer entre elles sans réappariement.

## Connexion d'une radio

1. Allumez votre radio Meshtastic.
2. Ouvrez l'application et appuyez sur l'onglet **Connect**.
3. L'application recherche automatiquement les appareils à proximité lorsque vous n'êtes pas connecté.
4. Appuyez sur le nom de votre appareil dans la liste pour vous connecter.

L'application se souvient de votre appareil préféré et se reconnecte automatiquement lorsque la radio est à portée.

## Déconnexion d'une radio

Balayez vers la gauche sur une radio connectée dans la vue Connecter et appuyez sur **Déconnecter**. La radio continue de fonctionner sur le maillage - elle cesse simplement de se synchroniser avec l'application.

## Activité en direct

Appuyez longuement sur une ligne radio connectée pour démarrer une activité en direct (iOS 16.2+). L'activité en direct affiche l'état du maillage sur votre écran de verrouillage et dans l'île dynamique.

## Gestion de plusieurs radios

Vous pouvez coupler plusieurs radios, mais une seule est active à la fois. Basculez entre eux en appuyant sur un autre appareil dans la vue Connecter.

## Force du signal BLE

L'application affiche la force du signal Bluetooth des appareils à proximité pendant la numérisation :

![BLE Signal Strength](../assets/screenshots/bleSignalStrength.png)

## Icônes d'état de la connexion

| Icône | Sens |
|------|---------|
| ![BLE connected](../assets/screenshots/btConnected.png) | Connecté via BLE |
| ![Reconnecting](../assets/screenshots/btReconnecting.png) | Reconnexion / tentative de reconnexion |
| ![TCP connected](../assets/screenshots/tcpConnected.png) | Connecté via TCP/IP |
| ![Serial connected](../assets/screenshots/serialConnected.png) | Connecté via série (macOS) |

## Diagnostic des anomalies

**Radio n'apparaît pas dans la liste**
- Assurez-vous que le Bluetooth est activé dans les paramètres iOS → Bluetooth.
- Déplacez-vous à moins de 10 mètres de la radio.
- Redémarrez la radio.

**Connexion tombe à plusieurs reprises**
- Vérifiez le niveau de la batterie sur la radio.
- Essayez d'oublier l'appareil dans les paramètres iOS → Bluetooth et se reconnecter.

**L'application demande l'autorisation Bluetooth**
- Accordez l'autorisation dans les paramètres iOS → Confidentialité et sécurité → Bluetooth → Meshtastic.

---

> **Tip — Radio connectée**
> Balayez vers la gauche pour vous déconnecter. Appuyez longuement pour démarrer l'activité en direct.

