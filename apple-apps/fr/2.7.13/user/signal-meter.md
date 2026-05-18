
# Comment fonctionne le compteur de signaux Meshtastic

![Signal meter levels](../assets/screenshots/signalMeter_full_all.png)

![Compact signal meter](../assets/screenshots/signalMeter_compact_all.png)

Le compteur de signal Meshtastic - souvent vu comme une série de barres ou une couleur d'état dans l'application - est calculé très différemment des "barres" sur un téléphone portable traditionnel ou un routeur Wi-Fi.

La plupart des appareils grand public ne mesurent que le volume "fort" d'un signal. Cependant, comme Meshtastic utilise la technologie **LoRa (Long Range)**, son compteur de signal utilise une logique qui mesure la **claireté** du signal, par rapport aux paramètres spécifiques que votre maillage utilise.

---

## 1. Les deux mesures : "Bruteur" vs. "Clarté"

Pour comprendre le compteur, vous devez comprendre les deux mesures que la puce radio LoRa prend chaque fois qu'elle reçoit un message :

* **RSSI (indicateur de force du signal reçu) :** C'est le **bruit** de la puissance brute qui frappe votre antenne.
* **SNR (rapport signal/bruit) :** C'est la **clarté** du signal par rapport à l'statique de fond.

> **Tip — L'analogie :**Imaginez que vous essayez d'entendre un ami vous parler.
> * **RSSI** est le volume de leur voix.
> * **Le bruit de plancher** est le bruit de fond dans la pièce (climatisation, autres personnes qui parlent, circulation).
> * **SNR** est la facilité avec laquelle vous pouvez distinguer la voix de votre ami du bruit de fond.

Si votre ami vous crie dessus lors d'un concert de rock assourdissant, le signal est incroyablement fort (High RSSI), mais vous ne pouvez toujours pas le comprendre parce que le bruit de fond est plus fort (Bad SNR). À l'inverse, si votre ami vous chuchote dans une bibliothèque silencieusement, le signal est très faible (Faible RSSI), mais vous pouvez parfaitement le comprendre (Grand SNR).

---

## 2. La magie de LoRa : entendre "Sous le plancher du bruit"

Pour les radios standard (comme FM ou Wi-Fi), si le bruit de fond est plus fort que le signal (un SNR négatif), le récepteur entend simplement l'électricité statique.

LoRa est spéciale. Il utilise la modulation **"Spread Spectrum"**, qui permet à la radio d'extraire mathématiquement un signal de l'air même lorsqu'il est enfoui profondément *sous* le bruit de fond. C'est pourquoi vous verrez fréquemment **nombres SNR négatifs** dans Meshtastic (par exemple, -10dB, ce qui signifie que le signal est 10 décibels plus faible que la statique d'arrière-plan).

Selon le préréglage Meshtastic que vous utilisez (par exemple,`LongFast`Contre.`ShortFast`), La radio a une **limite SNR** spécifique - la quantité maximale absolue de bruit qu'elle peut tolérer avant que le message ne soit complètement perdu à cause de l'électricité statique.

---

## 3. Comment le compteur de signal calcule la qualité

Les applications Meshtastic prennent à la fois RSSI et SNR et les exécutent à travers une formule spécifique pour attribuer à votre signal une note de qualité (Aucun, Mauvais, Juste ou Bon). Il met spécifiquement à l'échelle ces valeurs en fonction des limites physiques du préréglage radio que vous utilisez.

Voici exactement comment l'application décide combien de barres (ou quelle couleur) vous montrer :

| Niveau | Barres | Critères | Sens |
|-------|------|----------|---------|
| Bien | 3 | RSSI mieux que`-115 dBm`**ET** SNR au-dessus de la limite de base pour votre préréglage | Le signal est à la fois fort et clair - connexion saine. |
| Juste | 2 | Tombe entre le bon et le mauvais | Le signal devient plus silencieux ou plus bruyant, mais la radio comprend bien le message. |
| Mauvais | 1 | RSSI tombe à`-120 dBm`Ou pire, **OU** SNR dans`5.5 dB`Du point de rupture absolu de votre préréglage | À peine accroché - au bord de la portée ou des interférences lourdes. |
| Aucun | 0 | RSSI pire que`-126 dBm`**ET** SNR est tombé`7.5 dB`En dessous de la limite idéale | Transmission complètement enterrée dans l'électricité statique. |

---

## 4. Ce que cela signifie pour vous

Parce que le compteur de Meshtastic agit comme un **"Clarity Meter"**, il se comporte différemment de ce à quoi la plupart des gens s'attendent :

> **Tip — Ne paniquez pas à propos d'un faible RSSI :**Vous pourriez voir une valeur RSSI apparemment terrible comme`-118 dBm`. Sur un téléphone portable, vous auriez zéro barre. Mais si vous avez un SNR de`+2 dB`, Meshtastic montrera toujours un signal fort ! *La bibliothèque est calme, donc le murmure est parfaitement entendu. *

> **Warning — Méfiez-vous du bruit local :**Si vous branchez une antenne massive et voyez un excellent RSSI (par exemple,`-90 dBm`) Mais votre compteur de signal n'affiche que **1 Bar (Mauvais)**, vous avez un problème. Cela signifie que vous avez des interférences locales - peut-être une alimentation bon marché, un ordinateur bruyant ou une tour radio à proximité - créant tellement d'électricité statique qu'elle noie votre maillage.

