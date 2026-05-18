
# Carte et points de route

L'onglet Carte affiche tous les nœuds qui ont partagé une position, superposés à une couche de base Apple Maps.

## Épingles à nœud

Chaque nœud qui a signalé une position GPS apparaît sous la forme d'une épingle de cercle colorée sur la carte. La **ligne continue verte** montre un nœud directement connecté ; **lignes pointillées orange** montrent des nœuds atteints via le maillage. Une étoile violette marque un point de passage. Appuyez sur une épingle pour voir le nom du nœud, l'heure de la dernière fois entendue, les informations sur le signal et un raccourci pour envoyer un message direct.

Les épingles se mettent à jour automatiquement lorsqu'un nouveau paquet de position est reçu du maillage.

## Filtrer les nœuds sur la carte

Appuyez sur le **bouton de filtre** (icône d'entonnoir,`line.3.horizontal.decrease.circle`) Dans la barre d'outils en bas à droite pour ouvrir la feuille de filtres cartographiques. Lorsqu'un filtre est actif, l'icône apparaît **remplie** pour indiquer que le filtrage est en vigueur.

| Filtre | Description |
|--------|-------------|
| Via LoRa | Afficher uniquement les nœuds entendus directement sur la radio LoRa |
| Via MQTT | Afficher uniquement les nœuds pontés à travers MQTT |
| En ligne | Afficher uniquement les nœuds entendus au cours des 2 dernières heures |
| Crypté | Afficher uniquement les nœuds utilisant le cryptage PKI |
| Favoris | Afficher uniquement les nœuds que vous avez marqués comme favoris |
| Distance | Limite aux nœuds dans un rayon choisi de votre emplacement actuel |
| Saute Loin | Curseur de **Tous** à **7** - restreint par nombre de sauts (0 = direct seulement) |
| Rôles | Filtrer par un ou plusieurs rôles d'appareil (par ex. Routeur, client, répéteur) |

> **Tip — Vérification de la plage LoRa**
> Activez le filtre **Via LoRa** et désactivez **Via MQTT** pour ne voir que les nœuds accessibles directement par radio, ce qui est utile pour évaluer si un lien LoRa direct est réalisable.

## Couches de carte

Appuyez sur l'icône du calque (en haut à droite) pour basculer entre :

| Couche | Description |
|-------|-------------|
| Norme | Hybride rue/satellite Apple Maps par défaut |
| Satellite | Imagerie aérienne |
| Superpositions GeoJSON | Couches de cartes personnalisées chargées à partir de`.geojson`Fichiers dans le stockage de fichiers de l'application |

## Waypoints

Les waypoints sont des points d'intérêt nommés que vous pouvez partager à travers le maillage.

### Création d'un Waypoint

1. Appuyez longuement n'importe où sur la carte.
2. Entrez un nom, une description facultative et une icône de verrouillage (pour limiter l'édition au créateur).
3. Appuyez sur **Enregistrer** - le waypoint diffuse à tous les nœuds du canal principal.

### Modification d'un Waypoint

Touchez une épingle de waypoint existante, puis touchez **Modifier**. Modifie immédiatement la diffusion du maillage.

### Suppression d'un Waypoint

Appuyez sur le waypoint, puis sur **Supprimer**. La suppression se diffuse à tous les nœuds.

## Sentier de nœud

Lorsqu'un nœud a signalé plusieurs positions au fil du temps, une ligne de piste relie les positions historiques sur la carte, montrant le chemin du nœud.

## Votre emplacement

Votre position GPS actuelle apparaît sous la forme d'un point bleu (indicateur de localisation iOS standard). Activez la diffusion de position dans **Paramètres → Position** pour partager votre position avec le maillage.

