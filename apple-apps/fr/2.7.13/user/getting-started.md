
# Getting Started

Welcome to the Meshtastic Apple app — your iOS, iPadOS, macOS, watchOS, and visionOS companion for off-grid mesh radio communication.

## What You Need

- A supported Meshtastic LoRa radio device (see [meshtastic.org](https://meshtastic.org/docs/hardware/))
- iPhone, iPad, or Mac with the Meshtastic app installed
- Bluetooth enabled on your device

## Step 1: Power On Your Radio

Turn on your Meshtastic radio. Most devices show a splash screen and begin broadcasting on the mesh immediately.

## Step 2: Open the App and Connect

1. Open the Meshtastic app.
2. Appuyez sur **Connecter** (l'icône d'antenne dans la barre d'onglets).
3. Votre radio devrait apparaître dans la liste des appareils à proximité en quelques secondes.
4. Appuyez sur le nom de l'appareil pour vous connecter.

L'indicateur de connexion devient vert lorsque la radio est jumelée et communique.

> **Conseil :** Si votre appareil n'apparaît pas, assurez-vous que le Bluetooth est activé dans les paramètres iOS et que la radio est à portée (environ 10 mètres pour le jumelage initial).

## Étape 3 : Définissez votre nom et votre identifiant court

1. Allez dans **Paramètres → Utilisateur**.
2. Entrez un **Nom long** (votre nom d'affichage, jusqu'à 39 caractères).
3. Entrez un **Nom court** (jusqu'à 4 caractères ou un emoji - affiché dans le cercle de nœud).
4. Appuyez sur **Enregistrer**.

Votre nom est automatiquement diffusé aux nœuds à proximité.

## Étape 4 : Explorez le maillage

Une fois connectée, l'application affiche les nœuds à proximité dans l'onglet **Nœuds**. Appuyez sur n'importe quel nœud pour voir des détails tels que la force du signal, l'heure de la dernière écoute et la position.

Envoyez votre premier message en appuyant sur l'onglet **Messages** et en sélectionnant le canal principal.

## Étape 5 : Vérifiez vos paramètres

Visitez **Paramètres → LoRa** pour vérifier que votre code de région correspond à votre emplacement. L'utilisation de la mauvaise région est illégale et empêchera la communication avec d'autres nœuds de votre région.

## À propos de Meshtastic

![About Meshtastic](../assets/screenshots/aboutMeshtastic.png)

## Prochaines étapes

- [Connexion d'appareil Bluetooth](bluetooth.md) - gérer plusieurs radios
- [Messages et canaux](messages.md) - envoyer des émissions et des messages directs
- [Liste des nœuds](nodes.md) - comprendre les indicateurs d'état des nœuds
- [Map & Waypoints](map.md) - voir le maillage sur une carte

