
# Messages et canaux

Meshtastic utilise un système de canaux pour les diffusions de groupe et les messages directs pour les conversations privées en tête-à-tête.

## Canaux

### Historique des messages

Les conversations de canal chargent les **50 messages** les plus récents par défaut. Faites défiler vers le haut et appuyez sur **Charger plus** pour récupérer le lot suivant. Cela permet à l'application de rester réactive sur les canaux avec des milliers de messages.

### Index des canaux

| Symbole | Sens |
|--------|---------|
| **0** (cercle principal) | Canal principal - les paquets de diffusion sont envoyés ici. Les données de localisation sont diffusées à partir du premier canal où elles sont activées (firmware 2.7+). |
| **1–7** | Canaux secondaires - groupes de messagerie distincts, chacun sécurisé par sa propre clé. |

### Configuration du canal

![Channel form](../assets/screenshots/channelForm_primary.png)

Le formulaire de canal vous permet de configurer le nom du canal, la clé de cryptage, le rôle, le partage de position et les paramètres de liaison montante/descendante MQTT.

### Sécurité du canal

| Icône | Sens |
|------|---------|
| ![Securely Encrypted](../assets/screenshots/lockClosed.png) | **Crypté sécurisé** - la chaîne utilise une clé AES de 128 bits ou 256 bits. |
| ![Not Securely Encrypted](../assets/screenshots/lockOpen.png) | **Non crypté de manière sécurisée** - le canal n'utilise pas de clé ou une clé connue de 1 octet, mais n'est pas utilisé pour des données de localisation précises. |
| ![Insecure with Location](../assets/screenshots/lockOpenRed.png) | **Insécurisé avec l'emplacement** - le canal n'est pas crypté en toute sécurité et est utilisé pour des données de localisation précises. |
| ![Insecure with MQTT](../assets/screenshots/lockOpenMqtt.png) | **Peu sécurisé avec MQTT** - pas crypté en toute sécurité et les données de localisation précises sont liées à Internet via MQTT. |

---

> **Tip — Partager les chaînes**
> Un code QR contient la configuration LoRa et les canaux nécessaires à la communication des radios. Utilisez **Replacer les canaux** pour écraser ou **Ajouter des canaux** pour ajouter aux canaux existants.

> **Tip — Gérer les canaux**
> La chaîne principale gère le trafic de diffusion. Ajoutez des canaux secondaires pour des groupes de messagerie distincts, chacun sécurisé par sa propre clé.

> **Tip — Administration activée**
> Sélectionnez un nœud dans le menu déroulant pour gérer les appareils connectés ou distants.

---

## Messages directs

### Contacts

| Élément | Sens |
|---------|---------|
| ![Favorites](../assets/screenshots/favorite.png) | **Favoris** - les contacts et les nœuds favoris avec des messages récents apparaissent en haut de la liste des contacts. |
| ![Long press](../assets/screenshots/longPress.png) | **Appuyez longue sur Actions** - appuyez longuement pour ajouter à vos favoris ou désactiver le contact, ou supprimer une conversation. |

### Cryptage

![Encryption legend](../assets/screenshots/lockLegend.png)

| Icône | Sens |
|------|---------|
| ![Shared Key](../assets/screenshots/lockOpen.png) | **Clé partagée** - les messages directs utilisent la clé partagée pour la chaîne. |
| ![Public Key Encryption](../assets/screenshots/lockClosed.png) | **Cryptage à clé publique** - les messages directs utilisent l'infrastructure de clé publique pour le cryptage. Nécessite un micrologiciel 2.5 ou une version ultérieure. |
| ![PKI Mismatch](../assets/screenshots/keySlash.png) | **Public Key Mismatch** - la clé publique la plus récente pour ce nœud ne correspond pas à la clé précédemment enregistrée. Vérifiez avec qui vous envoyez des messages en comparant les clés publiques en personne ou par téléphone. |

---

### Réactions de tapback

Appuyez longuement sur n'importe quel message et appuyez sur **Tapback** pour envoyer une réaction emoji.

![Tapback input](../assets/screenshots/tapbackInput.png)

---

> **Tip — Messages**
> Envoyer des émissions de chaîne et des messages directs. Appuyez longuement sur n'importe quel message pour des actions telles que copier, répondre, taper et détails de livraison.

---

## Statut du message

![Message status reference](../assets/screenshots/ackErrors.png)

| Couleur | Sens |
|--------|---------|
| Gris | Livraison réussie. |
| Bulle orange | **Reconnu par un autre nœud** - le message a été relayé mais n'a pas été confirmé par le destinataire final. |

Les erreurs suivantes peuvent apparaître sur une bulle de message (rouge sauf indication contraire) :

| Statut | Description |
|--------|-------------|
| Pas d'itinéraire | Aucun itinéraire n'a été trouvé vers le nœud de destination. |
| J'ai NAK | Le nœud de destination a explicitement rejeté le message. |
| Temps de repos | Le message a été chronométré en attendant d'être accusé de réception. |
| Pas d'interface | L'interface radio n'est pas disponible. |
| Transmission maximale | Tentatives de retransmission maximales atteintes sans succès. |
| Pas de chaîne | Le canal spécifié n'existe pas sur la destination. |
| Trop grand | Le paquet dépasse la taille maximale autorisée. |
| Pas de réponse | Aucune réponse reçue de la destination. |
| L'ICP a échoué | Échec du cryptage/décryptage de l'infrastructure à clé publique. |
| Mauvaise demande | Paquet mal formé rejeté par la destination. |
| Non autorisé | Le nœud de destination a refusé la demande en raison d'autorisations. |

> Le gris indique une livraison réussie. L'orange indique une erreur réessayable. Le rouge indique un échec permanent qui ne réussira pas lors d'une nouvelle réessai.

---

## Apparence du lien

Liens dans les bulles de message - y compris les URL, les liens de canal Meshtastic et la démarque`[text](url)`Liens - sont stylisés avec un soulignement et les normes de conception Couleur du lien (Bleu 400). Cela rend les liens visuellement distincts du texte de message ordinaire en mode clair et sombre. Appuyer sur un lien l'ouvre dans le navigateur, ou pour les URL de canal/contact Meshtastic, ouvre le gestionnaire d'application approprié.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/messageText_link_dark.png">
  <img src="../assets/screenshots/messageText_link.png" alt="Message bubble with styled link">
</picture>

---

## Formatage des messages (iOS 18+)

Sur iOS 18 et versions ultérieures, les boutons de formatage apparaissent dans la barre d'outils compacte sous le champ de composition après avoir tapé au moins 3 caractères. Les boutons de formatage partagent la ligne de la barre d'outils avec la cloche d'alerte, la broche de position et le compteur d'octets, tous rendus sous forme d'icônes compactes. La barre d'outils défile horizontalement si elle dépasse la largeur de l'écran.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/composeArea_formatting_dark.png">
  <img src="../assets/screenshots/composeArea_formatting.png" alt="Compose area with formatting toolbar and live preview">
</picture>

### Styles pris en charge

| Bouton | Style | Syntaxe de démarque |
|--------|-------|-----------------|
| Symbole SF en gras | Gras | `**texte**` |
| Symbole SF en italique | Italique | `*texte*` |
| Symbole SF barré | Barré | `~~texte~~`` |
| Code Symbole SF | Code | `` `texte` `` |
| Symbole de lien SF | Lien | `[texte](url)` |

### Comment formater le texte

1. **Sélectionnez du texte et appuyez sur un bouton** - sélectionnez un mot ou une phrase dans le champ de composition, puis appuyez sur un bouton de formatage. Les délimiteurs de démarque appropriés sont insérés autour de la sélection. Tous les délimiteurs de démarque existants dans la sélection sont d'abord supprimés pour éviter le chevauchement de la syntaxe. L'espace blanc sur les bords de la sélection est déplacé à l'extérieur des délimiteurs afin que le marquage s'affiche correctement.
2. **Appuyez d'abord sur un bouton, puis tapez** - avec le curseur placé (pas de sélection), appuyez sur un bouton de formatage. Des délimiteurs sont insérés et le curseur est placé entre eux afin que vous puissiez saisir immédiatement du texte formaté.
3. **Désactiver** - sélectionnez le texte qui est déjà enveloppé de délimiteurs et appuyez sur le même bouton de formatage pour supprimer les délimiteurs.

### Aperçu en direct

Lorsque le champ de composition contient une syntaxe de démarque, une bulle d'aperçu apparaît au-dessus du champ de composition montrant à quoi ressemblera le message lorsqu'il sera envoyé. L'aperçu se met à jour en temps réel au fur et à mesure que vous tapez. Lorsqu'aucune démarque n'est présente, l'aperçu est masqué.

Le formatage Markdown est également rendu dans les aperçus de la liste des chaînes et des messages des utilisateurs, de sorte que vous pouvez voir le texte formaté en un coup d'œil.

| Exemple | Description |
|---------|-------------|
| ![Bold preview](../assets/screenshots/messagePreview_bold.png) | Aperçu montrant le formatage **gras** appliqué au texte. |
| ![Mixed preview](../assets/screenshots/messagePreview_mixed.png) | Aperçu montrant **bold**, *italic*, ~~strikethrough~~ et `code` formatage combiné. |

### Changement de style

Lorsque vous sélectionnez un texte qui contient déjà des délimiteurs de démarque et que vous appliquez un style différent, les délimiteurs existants sont supprimés et remplacés par le nouveau style. Par exemple, en sélectionnant`**bold**`Et en appuyant sur Strikethrough produit`~~bold~~`.

Après avoir appliqué un style, la sélection s'étend pour inclure les délimiteurs (par exemple, sélectionner`dolphin`Et en appuyant sur les sélections en gras`**dolphin**`), Ce qui facilite la désactivation ou le passage immédiat à un style différent.

### Sécurité de la sélection

Si votre sélection chevauche partiellement les délimiteurs existants, la sélection se développe automatiquement pour inclure le délimiteur complet exécuté avant le formatage. Tous les caractères délimiteurs orphelins (non appariés) laissés ailleurs dans le texte sont nettoyés automatiquement. Cela empêche les démarques brouillées comme`th***~~~~~~e~~`.

### Utilisateurs iOS 17

La barre d'outils de formatage n'est disponible que sur iOS 18 et versions ultérieures. Les utilisateurs sur iOS 17.x voient le champ de composition standard sans modification de leur expérience.

### Mac Catalyst

Sur Mac Catalyst, appuyer sur **Entrée** envoie le message. Appuyez sur **Shift+Enter** pour insérer un saut de ligne. Le bouton de la palette de caractères reste disponible à côté des boutons de formatage.

> **Tip — Limite de message**
> Les messages sont limités à 200 octets. Les délimiteurs de démarque comptent pour cette limite (par exemple,`**bold**`Utilise 4 octets supplémentaires pour le`**`Paires). Le compteur d'octets dans la barre d'outils affiche l'espace restant.

