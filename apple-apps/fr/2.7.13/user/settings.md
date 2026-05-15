
# Paramètres
L'onglet Paramètres vous permet de configurer l'application et votre radio Meshtastic connectée.
## Paramètres de l'application
Préférences générales de l'application, y compris le style de la carte, le comportement de notification et le thème. Ceux-ci n'affectent que l'application - pas la radio.
## Configuration radio
La configuration de la radio nécessite un nœud connecté. Sélectionnez votre nœud dans la section **Configurer** si vous avez plusieurs nœuds.
### LoRa
Les paramètres LoRa contrôlent la façon dont votre radio communique sur le maillage :
| Cadre | Description |
|---------|-------------|
| Région | Votre région géographique. **Doit être défini correctement** - l'utilisation de la mauvaise région est illégale et empêche la communication avec les nœuds locaux. Les régions disponibles comprennent l'ensemble standard plus le Népal 865 MHz, le Brésil 902 MHz, la région de l'UIT 1 Amateur 2m, la région de l'UIT 2/3 Amateur 2m et les bandes étroites de l'UE 866 / 874 / 917 / 868. |
| Préréglage du modem | Compromis vitesse/range. La plupart des utilisateurs devraient utiliser Long Fast ou Long Slow. |
| Limite de saut | Le nombre de fois où un message est répété par d'autres nœuds. Des valeurs plus élevées augmentent la portée, mais aussi le trafic de maillage. |
| Créneau de fréquence | Affinez la fréquence exacte dans votre région. |

### Canaux
Gérez jusqu'à 8 canaux (0–7). Le canal 0 est le canal de diffusion principal. Des canaux supplémentaires créent des groupes de messagerie isolés avec leurs propres clés de cryptage.
### Sécurité
Configurez le cryptage PKI (Public Key Infrastructure) pour les messages directs. Nécessite un firmware 2.5+.
### Utilisateur
Définissez votre nom long (nom d'affichage) et votre nom court (identifiant à 4 caractères/emoji affichés dans le cercle de nœud).
### Bluetooth
Paramètres radio BLE, y compris le mode PIN et l'économie d'énergie. Des modifications s'appliquent au prochain redémarrage de la radio.
### Appareil
Rôle de l'appareil, sortie série, flux de journaux de débogage et intervalle de diffusion d'informations sur les nœuds.
### Affichage
Délai d'attente de l'écran, carrousel automatique des écrans, écran rabattable pour des orientations de montage alternatives et contraste OLED.
### Réseau
SSID/mot de passe Wi-Fi pour la connexion TCP, le serveur NTP et Ethernet (matériel pris en charge uniquement).
### Position
Intervalle de mise à jour GPS, précision de la position et diffusion intelligente de la position. Activez **Position de diffusion** pour partager votre emplacement avec le maillage.
### Puissance
Profils d'économie de batterie, modes de veille et temps d'éveil minimum. Critique pour les nœuds de routeur à énergie solaire.
## Configuration du module
Modules de fonctionnalités optionnels. Uniquement disponible lorsque votre nœud connecté prend en charge le module.
| Module | Description |
|--------|-------------|
| Éclairage ambiant | Contrôlez l'éclairage NeoPixel/LED sur le matériel pris en charge. |
| Messages En Conserve | Raccourcis de messages préprogrammés accessibles à partir des boutons de l'appareil. |
| Capteur de détection | Configurez les capteurs de mouvement ou de contact PIR. |
| Notification externe | Avertisseur ou alertes LED pour les messages entrants. |
| MQTT | Messages de liaison montante/descendante à un courtier MQTT pour le pont Internet. |
| Test de portée | Test de portée automatisé avec journalisation de position. |
| Comptoir Pax | Comptage anonyme du trafic piétonnier via la détection de sonde Bluetooth/Wi-Fi. |
| Sonnerie | Mélodies RTTTL personnalisées pour les tonalités de notification. |
| Stocker et transférer | Stockez les paquets pour les nœuds qui sont temporairement hors ligne. |
| Série | Sortie série UART pour l'intégration avec d'autres matériels. |
| Télémétrie | Rapports sur les capteurs de qualité de l'appareil, de l'environnement et de l'air. |

## Mises à jour du micrologiciel
Vérifiez et appliquez les mises à jour du micrologiciel OTA à votre radio connectée directement depuis l'application. Voir [Mises à jour du firmware](firmware.md) pour plus de détails.
## Traduction automatique de la documentation
Sur les appareils exécutant iOS 26 ou version ultérieure, la documentation intégrée à l'application est automatiquement traduite dans la langue de votre appareil lorsqu'elle diffère de l'anglais.
### Comment ça marche
- **Détection de langue** : L'application lit le paramètre de langue principale de votre appareil chaque fois que vous ouvrez une page de documentation.- **Traduction sur l'appareil** : Les pages sont traduites à l'aide du framework de traduction sur l'appareil d'Apple (iOS 26+). Si une langue n'est pas prise en charge par le cadre de traduction, l'application revient au modèle Foundation sur l'appareil (iOS 26+ uniquement).- **Aucun réseau requis** : Après la traduction initiale, tout le contenu est disponible hors ligne.- **Mise en cache** : Les pages traduites sont stockées localement afin qu'elles se chargent instantanément lors des visites suivantes.- **Background prefetch** : Une fois la page actuelle traduite, les pages restantes sont traduites en arrière-plan à faible priorité.
### Retour à l'anglais
Si la traduction n'est pas disponible (plus ancienne qu'iOS 26, langue non prise en charge ou pack de langues non téléchargé), la documentation originale en anglais est affichée. L'application ne montre jamais les pages vierges ou cassées.
### Gestion du cache
- Les fichiers traduits sont stockés dans le support d'application et persistent dans les lancements d'applications.- Une limite de 50 Mo par langue est appliquée à l'aide de l'expulsion la moins récemment utilisée.- Lorsque la documentation source en anglais est mise à jour (nouvelle version de l'application), les traductions radites sont automatiquement régénérées.
> **Conseil - Changement de langue** : Si vous changez la langue de votre appareil pendant que l'application est ouverte, les pages de documentation se rechargent automatiquement dans la nouvelle langue.
