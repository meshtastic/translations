
# Intégration TAK
L'application Meshtastic prend en charge l'intégration Team Awareness Kit (TAK), permettant l'interopérabilité avec ATAK (Android Team Awareness Kit), iTAK et d'autres systèmes compatibles CoT (Cursor-on-Target) sur la radio maillée LoRa - aucun cellulaire ou Internet requis.
## Qu'est-ce que TAK ?
TAK est une plate-forme de sensibilisation à la situation largement utilisée dans les contextes tactiques, de gestion des urgences et de loisirs en plein air. Il affiche les positions et le statut des membres de l'équipe sur une carte partagée. Meshtastic relie les utilisateurs de TAK à LoRa, de sorte que les équipes restent connectées sans avoir besoin d'une couverture cellulaire ou Internet.
## Rôles d'appareil pris en charge
L'intégration TAK fonctionne avec deux rôles d'appareil :
| Icône | Rôle | Description |
|------|------|-------------|
| ![TAK](../assets/screenshots/roleTak.png) | TAK | Rôle TAK complet - envoie des rapports de position CoT et peut relayer les paquets de données TAK. |
| ![TAK Tracker](../assets/screenshots/roleTakTracker.png) | Traqueur TAK | Rôle TAK léger à position seule. Consommation d'énergie plus faible, pas de relais de paquets. |

Définissez le rôle de l'appareil dans **Paramètres → Appareil**.
> **Conseil — Version du micrologiciel**> Le format complet du fil TAK V2 (formes, itinéraires, marqueurs, casevac, urgence) nécessite un firmware **2.8.0 ou ultérieur** sur la radio connectée. L'ancien micrologiciel prend toujours en charge PLI et GeoChat par rapport au format V1 hérité - l'application recule automatiquement.
## Écran du serveur TAK
**Paramètres → Le serveur TAK** est la destination unique pour tout ce qui concerne TAK. L'écran est organisé de haut en bas afin que vous puissiez configurer votre identité, démarrer le serveur et coupler un client ATAK / iTAK en un seul passage.
### Identité TAK
La première section, **TAK Identity**, contrôle l'équipe au niveau du micrologiciel et l'identité du rôle que la radio attache à chaque rapport de position :
| Cadre | Description |
|---------|-------------|
| Équipe | La couleur de l'équipe montrée aux clients TAK. La valeur par défaut est Cyan ; toutes les couleurs standard de l'équipe ATAK sont disponibles. |
| Rôle | Votre rôle TAK. Les choix sont Membre de l'équipe (par défaut), chef d'équipe, QG, Sniper, Medic, Forward Observer, RTO et K9. |

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/takIdentitySection_dark.png">
  <img src="../assets/screenshots/takIdentitySection.png" alt="TAK Identity section with Team and Role pickers">
</picture>

Un bouton **Enregistrer l'identité TAK** apparaît dans la section uniquement lorsqu'il y a des modifications non enregistrées. L'enregistrement envoie un message d'administration au nœud connecté ; vous verrez le changement reflété dans les clients TAK sur le prochain rapport de position.
> **Conseil - Sélecteurs d'identité désactivés ? **> Les sélecteurs restent désactivés jusqu'à ce que la radio connectée signale sa configuration du module TAK à l'application. Cela se produit généralement quelques secondes après la connexion - donnez-lui un moment, ou déconnectez-vous et reconnectez-le s'il n'apparaît pas.
### Statut du serveur, Activation et Canal
Sous la section identité :
- Un **indicateur d'état** montrant si le serveur TAK dans l'application est en cours d'exécution et si votre canal principal est adapté à l'utilisation de TAK.- Une bascule **Activer le serveur TAK**.- Un **sélecteur de canaux** pour le canal LoRa, le serveur relie les clients TAK et le maillage.- **Mode lecture seule** (traitez l'application comme un observateur TAK qui ne transmet pas CoT au maillage) et bascule **mesh-to-CoT relay**.
> **Conseil - Exigences du canal principal**> Le serveur TAK fonctionne en **mode lecture seule** jusqu'à ce que votre canal principal ait un nom non par défaut et une clé de cryptage 256 bits non par défaut. Utilisez le bouton **Fix Channel** sur la bannière d'avertissement pour appliquer un préréglage TAK recommandé (Short Fast, nouvelle touche AES, nom "TAK") en un seul clic.
### Certificats
Importez un paquet P12 (PKCS#12) ou PEM pour les connexions ATAK / iTAK protégées par mTLS. L'application stocke les certificats cryptés dans le trousseau - ils ne sont pas visibles par d'autres applications ou pour le partage de fichiers iTunes/Finder.
### Paquet de données
Exportez un zip de paquet de données TAK que vous pouvez charger dans ATAK / iTAK. Le client l'utilise pour trouver et faire confiance au serveur local de l'application sans entrer manuellement un hôte, un port ou un certificat.
## Itinéraires de réception
Lorsqu'un autre nœud sur le maillage envoie une route CoT (`b-m-r`), l'application l'écrit automatiquement en tant que paquet de données KML à `Documents/TAK Routes/` et publie une notification iOS afin que vous ne la manquiez pas :
| Champ | Contenu |
|-------|---------|
| Titre | Itinéraire reçu |
| Sous-titre | _Insigne d'appel d'itinéraire_ (ou "itinéraire inconnu") |
| Corps | Enregistré dans Fichiers → Meshtastic → TAK Routes. Ouvrir dans iTAK pour importer. |

iTAK ignore silencieusement la route que CoT a reçue sur sa connexion de streaming TCP, de sorte que ce repli vous permet d'importer la route manuellement. Appuyez sur la notification, puis dans Fichiers, accédez à **Sur mon iPhone → Meshtastic → TAK Routes**, partagez le `.zip` avec iTAK et choisissez **Importer le paquet de mission**.
> **Conseil - Où sont mes itinéraires ? **> Le dossier `TAK Routes` est créé la première fois qu'un itinéraire arrive. Si vous ne le voyez pas, aucun itinéraire n'a encore été reçu. Le KML à l'intérieur du zip est un KML 2.2 LineString standard - vous pouvez également l'ouvrir dans Google Earth ou dans n'importe quelle visionneuse KML.
## Comment ça marche sous le capot
Vous n'avez pas besoin de configurer quoi que ce soit : l'application choisit automatiquement le meilleur format de fil TAK que votre radio prend en charge. Firmware 2.8.0+ utilise le nouveau format V2 avec compression zstd-dictionnaire pour les types de messages plus riches et les transmissions LoRa plus courtes. L'ancien firmware continue d'utiliser le format V1 hérité, qui transporte PLI et GeoChat entre deux nœuds, ainsi qu'un repli Apple à Apple plus riche pour les formes, les marqueurs et les itinéraires.
Les développeurs et les utilisateurs curieux peuvent lire tous les détails du protocole dans [TAK Protocol](../developer/tak-protocol.html).
## Dépannage
**Le client TAK ne se connecte pas**- Assurez-vous que le serveur TAK intégré à l'application est activé dans **Paramètres → Serveur TAK**.- Confirmez que votre canal principal a un nom non-par défaut **et** clé de cryptage - sinon le serveur fonctionne en mode lecture seule. Utilisez **Fix Channel** dans la bannière d'avertissement si affichée.- Pour les clients mTLS, confirmez qu'un paquet P12 / PEM a été importé sous **Certificats**.
**Les itinéraires n'apparaissent pas dans iTAK**- ITAK ignore délibérément la route CoT du streaming TCP. Ouvrez le zip enregistré à partir de **Files → Meshtastic → TAK Routes** et importez-le en tant que paquet de mission.- Si le dossier `TAK Routes` est manquant, aucun CoT de route n'est encore arrivé.
**Sélecteurs d'identité sont désactivés**- La radio doit signaler la configuration de son module TAK à l'application avant que les sélecteurs ne l'activent. Reconnectez-vous s'il ne passe pas en quelques secondes.- Le nœud connecté doit avoir le rôle d'appareil **TAK** ou **TAK Tracker** - Équipe / Rôle n'a aucun effet sur les autres rôles.
## Exigences
- Firmware **2.3 ou version ultérieure** sur votre radio pour TAK PLI / GeoChat de base ; **2.8.0 ou version ultérieure** pour le format de fil TAK V2 complet.- Une application client compatible ATAK / iTAK / TAK sur votre téléphone ou votre tablette.- Appareil configuré avec le rôle **TAK** ou **TAK Tracker**.
