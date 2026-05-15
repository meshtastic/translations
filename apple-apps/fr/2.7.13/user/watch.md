
# App Apple Watch
L'application Meshtastic Apple Watch est un compagnon de l'application iPhone qui met deux fonctionnalités sur votre poignet : une **boussole Foxhunt** pour la recherche de direction radio et un **panneau de connectivité téléphonique** pour confirmer que votre montre est synchronisée.
Les données des nœuds sont automatiquement poussées vers la montre chaque fois que l'application iPhone est à portée via WatchConnectivity. Aucune connexion Bluetooth à votre radio Meshtastic n'est requise sur la montre elle-même.
## Exigences
| Besoin | Détails |
|-------------|---------|
| Apple Watch | watchOS jumelé avec iPhone |
| Application iPhone | Application iPhone Meshtastic ouverte et connectée à une radio |
| Emplacement | Services de localisation de la montre activés pour la recherche de directions |
| Proximité | Montre et iPhone dans la portée normale Bluetooth/Wi-Fi l'un de l'autre |

## Onglets
L'application Watch utilise une mise en page verticale. Balayez vers le haut ou vers le bas pour basculer entre les deux onglets.
### Chasse au renard
L'onglet Foxhunt répertorie les nœuds de maillage qui se trouvent à moins de **½ mile (≈ 800 m)** de l'emplacement actuel de votre montre et qui ont une position GPS connue. Les nœuds marqués comme cibles foxhunt à partir de l'application iPhone apparaissent toujours en haut de la liste, quelle que soit la distance.
Chaque ligne montre :
| Élément | Sens |
|---------|---------|
| Cercle coloré | Nom court du nœud, couleur dérivée du numéro du nœud |
| Nom | Nom long de nœud |
| Distance | Distance par rapport à votre emplacement actuel, codée par couleur par proximité |
| Flèche | Mini flèche portant pointant vers le nœud, tourne avec votre en-tête |

Appuyez sur n'importe quelle ligne pour ouvrir la boussole **Foxhunt** pour ce nœud.
#### Boussole Foxhunt
La boussole pointe vers le nœud sélectionné à l'aide du capteur de direction de votre montre. Il est conçu pour la recherche de direction radio (foxhunting) - marchez jusqu'à ce que la flèche pointe droit et que la distance indique zéro.
| Élément | Sens |
|---------|---------|
| Cadran rotatif | Les directions cardinales (N/NE/E...) tournent avec votre titre physique |
| Triangle orange | Indicateur nord fixe en haut de l'anneau |
| Flèche colorée | Flèche de roulement pointant vers le nœud cible |
| Cône de direction | Coin translucide soulignant la direction de la cible |
| Cercle central | Directions actuelles en degrés, par rapport à la cible et distance |
| Cercle de nœud | Badge de nom court du nœud cible |

**Codage couleur distance :**
| Couleur | Distance |
|--------|----------|
| Rouge | Loin (> 2⁄3 de ½ mile) |
| Jaune | Milieu de gamme (1⁄3 - 2⁄3 de ½ mile) |
| Vert | Fermer (< 1⁄3 de ½ mile) |

**Commentaire haptique :** La montre tape sur votre poignet lorsque vous faites face à moins de 10° du roulement du nœud cible - utile lorsque vous ne pouvez pas regarder l'écran.
### Téléphone
L'onglet Téléphone affiche l'état de connectivité entre votre montre et l'app iPhone qui l'accompagne.
| État | Sens |
|-------|---------|
| Téléphone connecté (vert) | L'application iPhone est accessible ; nombre de nœuds affiché |
| Téléphone non disponible | La montre est hors de portée ou l'application iPhone ne fonctionne pas |

Appuyez sur **Actualiser** pour demander une liste de nœuds mise à jour à partir de l'application iPhone. Si le téléphone est temporairement inaccessible, la montre revient aux données de nœud les plus récemment reçues.
## Définition de cibles de chasse au renard
Depuis l'application iPhone, marquez un nœud comme une cible de chasse au renard à partir de sa vue détaillée. Les nœuds marqués sont poussés vers la montre et épinglés en haut de la liste Foxhunt, quelle que soit la distance - utile lorsque vous savez quel nœud vous chassez avant d'être à un rayon de ½ mile.
