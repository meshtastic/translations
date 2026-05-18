
# Traduire l'application

La contribution des traductions à l'application Meshtastic Apple permet de rendre le projet accessible à un public plus large. Le processus est conçu pour être simple : un script génère des traductions automatiques sur votre Mac à l'aide d'Apple Intelligence, les marque pour examen et ouvre automatiquement une demande de tirage. Un locuteur natif examine et approuve ensuite les chaînes dans Xcode avant qu'elles ne soient expédiées.

## Exigences

Avant de commencer, assurez-vous d'avoir :

- **MacOS 26 ou version ultérieure** avec Apple Silicon
- **Apple Intelligence activé** - Paramètres système → Apple Intelligence et Siri
- **[Local-localizer](https://github.com/JoshuaSullivan/local-localizer)** installé (voir ci-dessous)
- **GitHub CLI** installé —`brew install gh`Et`gh auth login`

### Installer local-localizer

```bash
git clone https://github.com/JoshuaSullivan/local-localizer.git ~/local-localizer
cd ~/local-localizer && swift build -c release
mkdir -p ~/bin && cp .build/release/local-localizer ~/bin/local-localizer
```

Assurez-vous`~/bin`Est sur votre CHEMIN (ajouter`export PATH="$HOME/bin:$PATH"`À votre profil shell si nécessaire).

## Ajouter ou compléter une langue locale

Clonez le référentiel, puis exécutez le script de traduction avec votre code local :

```bash
git clone https://github.com/meshtastic/Meshtastic-Apple.git
cd Meshtastic-Apple
scripts/translate-locale.sh <locale>
```

Par exemple :

```bash
scripts/translate-locale.sh fr          # French
scripts/translate-locale.sh de formal   # German, formal register
scripts/translate-locale.sh ja polite   # Japanese, polite register
scripts/translate-locale.sh zh-Hant-TW  # Traditional Chinese (Taiwan)
```

Le script va :

1. Comptez combien de chaînes manquent ou doivent être mises à jour pour les paramètres régionaux
2. Générez un glossaire qui garde les termes de la marque Meshtastic (LoRa, MQTT, BLE, TAK, etc.) non traduits
3. Exécutez local-localizer à l'aide d'Apple Intelligence sur l'appareil - aucune clé Internet ou API n'est nécessaire
4. Marquez chaque nouvelle chaîne comme **Besoin d'examen** afin que les locuteurs natifs sachent les vérifier
5. Commit le résultat et ouvrez une demande d'extraction automatiquement

L'étape de traduction s'exécute entièrement sur votre appareil et prend environ 10 à 20 minutes pour une région complète.

## Options de tonalité

| Ton | Quand utiliser |
|---|---|
| `professional` | Par défaut - clair et neutre, adapté à la plupart des langues |
| `formal` | Recommandé pour l'allemand (`de`), Français (`fr`), Italien (`it`), Espagnol (`es`) - Sélectionne la forme polie de la deuxième personne (Sie / vous / Lei / usted) |
| `polite` | Recommandé pour les Japonais (`ja`) Et coréen (`ko`) - Sélectionne les formes verbales polies |
| `informal` | Registre occasionnel |
| `neutral` | Simple, pas de préférence d'enregistrement |

## Révision des chaînes traduites

Une fois le PR ouvert, tout locuteur natif peut consulter les traductions directement dans Xcode :

1. Ouvert`Meshtastic.xcworkspace`
2. Sélectionner`Localizable.xcstrings`Dans le navigateur de projet
3. Filtrez par votre région et définissez le filtre d'état sur **Besoin d'un examen**
4. Lisez chaque chaîne en contexte, modifiez-la si nécessaire et marquez-la **Révisé**
5. Poussez vos modifications à la branche PR

## Traduction automatique de la documentation

Sur les appareils exécutant iOS 26 ou une version ultérieure, la documentation intégrée à l'application est automatiquement traduite dans la langue de votre appareil lorsque vous ouvrez **Aide et documents**. Le pipeline de traduction fonctionne comme suit :

1. L'application lit les fichiers sources de démarquement anglais groupés.
2. Les segments de texte sont traduits à l'aide du cadre de traduction d'Apple, et se réfèrent aux modèles de fondation sur l'appareil si votre langue n'est pas prise en charge.
3. La démarque traduite est mise en cache localement afin que les visites suivantes se chargent instantanément.

Une fois que toutes les pages sont traduites en arrière-plan, l'application télécharge anonymement les fichiers traduits dans le référentiel [meshtastic/translations](https://github.com/meshtastic/translations) pour examen et amélioration de la communauté.

> **Tip — Utilisateurs anglais**
> Si la langue de votre appareil est l'anglais, aucune traduction ne se produit et la documentation en anglais fournie est affichée directement.

Merci d'avoir aidé à élargir la portée de Meshtastic !

