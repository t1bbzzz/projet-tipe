# Marégraphe DIY — Guide complet pour le TIPE

## Contexte

Ce guide accompagne la réalisation d'un marégraphe (capteur de niveau d'eau) à ultrasons basé sur l'article de Bresnahan et al. (2023) publié dans *Oceanography*. Le circuit reproduit fidèlement la version « North Carolina » de l'article, utilisant un Adafruit Feather M0 Adalogger comme microcontrôleur principal.

---

## 1. Composants électroniques : rôle et description détaillée

Chaque composant joue un rôle précis dans le fonctionnement du marégraphe. Voici une description détaillée de chacun, avec une indication claire de ce qui nécessite de la soudure et ce qui se branche simplement.

### 1.1. Adafruit Feather M0 Adalogger — Le cerveau (~22 €)

C'est le microcontrôleur, autrement dit le mini-ordinateur qui fait tourner tout le système. Il exécute le code Arduino que tu uploades, lit les mesures du capteur ultrasonique, écrit les données sur la carte SD, affiche les informations sur l'écran OLED, et gère l'horloge. Il intègre déjà un lecteur de carte microSD, ce qui évite d'ajouter un module supplémentaire. Il fonctionne en 3.3V et possède un connecteur JST pour brancher directement une batterie LiPo, avec un circuit de recharge intégré via USB.

**🔴 Soudure nécessaire : OUI** — Il faut souder les **stacking headers** (barrettes de broches) sur la carte. Ce sont des rangées de pins mâles/femelles qui permettent d'empiler les FeatherWings par-dessus. C'est la soudure la plus importante du projet (environ 32 points de soudure, 16 de chaque côté). Prends ton temps, c'est répétitif mais pas difficile.

*Où acheter : Mouser.fr, GoTronic.fr, Adafruit.com*

### 1.2. MaxBotix MB1040 LV-MaxSonar-EZ4 — Le capteur (~30 €)

C'est le capteur de distance à ultrasons, le cœur de la mesure. Il émet des ondes sonores à 42 kHz (inaudibles) vers la surface de l'eau, puis mesure le temps que met l'écho à revenir. À partir de ce temps, il calcule la distance capteur–eau. Le principe est similaire à celui d'un sonar ou d'un échosondeur océanographique, ce qui est très pertinent pour ton TIPE. Le modèle EZ4 a été choisi dans l'article pour son faisceau étroit, qui réduit les interférences avec les objets voisins (piliers de quai, etc.).

Caractéristiques clés : portée de 15 à 645 cm, résolution de 2.5 cm, précision de 5 cm (améliorable à ~1.5 cm après calibration), fréquence max de 20 Hz, consommation de ~2 mA.

**🔴 Soudure nécessaire : OUI** — Il faut souder **3 fils** entre le capteur et le Feather. C'est tout. Un fil rouge pour l'alimentation (pin 5V du capteur → pin 3V3 du Feather), un fil noir pour la masse (GND → GND), et un fil bleu pour le signal analogique (pin AN → pin A1). Ce sont les seules soudures de connexion du projet.

*Où acheter : Mouser.fr, Adafruit.com, Robotshop.com*

### 1.3. Adafruit FeatherWing OLED 128×32 — L'écran (~16 €)

C'est un petit écran OLED monochrome de 128×32 pixels qui se place au-dessus du Feather. Il affiche en temps réel la distance mesurée, l'heure, et le niveau de batterie. Très utile pendant la phase de test et de calibration pour vérifier que tout fonctionne sans avoir besoin d'un ordinateur. Pendant le déploiement terrain, il n'est pas indispensable mais permet de vérifier rapidement l'état du capteur. Il communique avec le Feather via le bus I2C (protocole de communication série avec seulement 2 fils : SDA et SCL).

**🔴 Soudure nécessaire : OUI** — Il faut souder des **stacking headers** sur cette carte aussi (mêmes barrettes que pour le Feather). Une fois soudées, la carte s'empile simplement sur le Feather, sans fil supplémentaire. Les connexions I2C passent automatiquement par les headers.

*Où acheter : Mouser.fr, GoTronic.fr, Adafruit.com*

### 1.4. Adafruit FeatherWing RTC (DS3231) — L'horloge (~10 €)

C'est une horloge temps réel (Real-Time Clock) de haute précision basée sur le chip DS3231. Elle garde l'heure même quand le Feather est éteint, grâce à une petite pile bouton intégrée (CR1220). Sans RTC, le Feather perdrait l'heure à chaque coupure de courant et les horodatages de tes mesures seraient faux. L'article de Bresnahan note aussi que l'ajout de la RTC a permis d'utiliser des modes de veille basse consommation entre les mesures, réduisant considérablement la consommation.

**🔴 Soudure nécessaire : OUI** — Même principe : souder des **stacking headers** ou des **headers simples** (si c'est la carte du dessus). Pas de fil supplémentaire, tout passe par l'empilement.

*Où acheter : Mouser.fr, GoTronic.fr, Adafruit.com*

### 1.5. Carte MicroSD 8 Go — Le stockage (~5 €)

Une simple carte mémoire MicroSD (8 Go suffit largement, le capteur n'enregistre que ~5.6 Ko par jour soit ~2 Mo par an). Elle stocke le fichier CSV avec toutes les mesures horodatées. Après le déploiement, tu retires la carte, tu la mets dans un lecteur SD branché à ton PC, et tu récupères le fichier pour l'analyser avec Python.

**🟢 Soudure nécessaire : NON** — La carte s'insère simplement dans le slot microSD du Feather M0 Adalogger.

*Où acheter : Amazon.fr, n'importe quel revendeur*

### 1.6. Batterie LiPo 3.7V 6600 mAh (connecteur JST) — L'alimentation (~25 €)

Une batterie lithium-polymère rechargeable qui alimente tout le système en autonomie. Le connecteur JST 2 broches se branche directement sur le Feather. Avec 6600 mAh et une consommation moyenne de ~5.5 mA, l'autonomie est d'environ 50 jours. Le Feather intègre un chargeur : quand tu branches le câble USB, la batterie se recharge automatiquement.

**⚠️ ATTENTION** : la batterie doit avoir un connecteur JST-PH 2 broches compatible Adafruit. Vérifier la polarité ! Adafruit utilise une polarité inverse par rapport à certains fabricants chinois.

**🟢 Soudure nécessaire : NON** — Le connecteur JST se branche directement.

*Où acheter : Adafruit.com, Amazon.fr (vérifier compatibilité JST)*

### 1.7. Stacking headers (×3 jeux) — La connectique (~4 €)

Ce sont des barrettes de broches spéciales qui ont des pins mâles en dessous et des trous femelles au-dessus, permettant d'empiler les cartes les unes sur les autres comme un mille-feuille. Chaque jeu comprend une barrette 16 broches et une barrette 12 broches. Il en faut 3 jeux : un pour le Feather, un pour la RTC, et un pour l'OLED (ou des headers simples pour la carte du dessus).

**🔴 Soudure nécessaire : OUI** — C'est la majorité du travail de soudure du projet. Environ 56 points de soudure au total pour les 2 barrettes × 3 cartes. Astuce : insère les headers dans une breadboard pour les maintenir droits pendant la soudure.

*Où acheter : Adafruit.com, Mouser.fr, GoTronic.fr*

### 1.8. Fils rigides 22 AWG (noir, rouge, bleu) — Les 3 connexions (~3 €)

Trois petits morceaux de fil électrique rigide (environ 10–15 cm chacun) pour relier le capteur MB1040 au Feather. Le fil rigide (solid-core) est préférable au fil multibrins pour la soudure sur des pins de circuit imprimé.

**🔴 Soudure nécessaire : OUI** — Chaque fil est soudé d'un côté sur une pin du Feather (ou du FeatherWing) et de l'autre sur une pin du MB1040. Total : 6 points de soudure.

*Où acheter : Adafruit.com, GoTronic.fr, Amazon.fr*

### 1.9. Câble USB-A vers Micro-USB — La programmation (~3 €)

Pour connecter le Feather à ton ordinateur afin d'uploader le code Arduino et de lire les messages du Serial Monitor. Tu en as probablement déjà un. Attention : il faut un câble de **données**, pas un câble de charge seule (certains câbles USB bon marché ne transmettent pas les données).

**🟢 Soudure nécessaire : NON** — Simple branchement USB.

### 1.10. Soudure sans plomb (~10 €)

Fil de soudure pour assembler le tout. Prendre du sans plomb par précaution.

### 1.11. Breadboard demi-taille (optionnel, ~5 €)

Utile pendant la phase de test pour connecter les composants sans soudure et expérimenter. Également très pratique pour maintenir les stacking headers droits pendant la soudure.

**🟢 Soudure nécessaire : NON** — C'est justement le principe de la breadboard.

---

## 2. Tableau récapitulatif des achats

| Composant | Rôle | Soudure ? | Prix |
|-----------|------|-----------|------|
| Adafruit Feather M0 Adalogger | Microcontrôleur + lecteur SD | 🔴 OUI (headers) | ~22 € |
| MaxBotix MB1040 LV-MaxSonar-EZ4 | Capteur distance ultrasons (42 kHz) | 🔴 OUI (3 fils) | ~30 € |
| FeatherWing OLED 128×32 | Écran affichage temps réel (I2C) | 🔴 OUI (headers) | ~16 € |
| FeatherWing RTC (DS3231) | Horloge temps réel (garde l'heure) | 🔴 OUI (headers) | ~10 € |
| Carte MicroSD 8 Go | Stockage données (fichier CSV) | 🟢 NON (insertion) | ~5 € |
| Batterie LiPo 3.7V 6600 mAh (JST) | Alimentation autonome | 🟢 NON (connecteur) | ~25 € |
| Stacking headers (×3 jeux) | Empiler les cartes | 🔴 OUI (~56 pts) | ~4 € |
| Fil rigide 22 AWG (3 couleurs) | 3 fils capteur → Feather | 🔴 OUI (6 pts) | ~3 € |
| Soudure sans plomb | Assemblage | — | ~10 € |
| Câble USB Micro-B | Programmation | 🟢 NON | ~3 € |
| Breadboard (optionnel) | Prototypage / aide soudure | 🟢 NON | ~5 € |

**Budget total estimé : ~120 à 150 €** (hors fer à souder et boîtier de récupération)

> **Conseil d'achat** : Commande tout sur le même site (Mouser.fr ou GoTronic.fr) pour économiser les frais de port. Vérifie aussi Amazon.fr pour la batterie et la carte SD.

---

## 3. Résumé de la soudure : ce qu'il faut souder

Le projet nécessite de la soudure, mais rien de très compliqué. Voici le détail :

**Étape 1 — Stacking headers sur le Feather M0**
Souder une barrette 16 broches et une barrette 12 broches sur le Feather. Astuce : insère les headers dans la breadboard, pose le Feather dessus, puis soude. Cela garantit que les pins sont parfaitement droites. **32 points de soudure.**

**Étape 2 — Stacking headers sur la FeatherWing RTC**
Même opération. La RTC se placera au milieu de l'empilement. **~28 points de soudure.**

**Étape 3 — Headers sur la FeatherWing OLED**
Comme c'est la carte du dessus, tu peux utiliser des headers simples (mâle) au lieu de stacking headers. **~28 points de soudure.**

**Étape 4 — Les 3 fils vers le capteur MB1040**
Souder 3 fils entre le capteur et l'un des FeatherWings (les pins sont partagées) : fil rouge (3V3 → pin 6), fil noir (GND → pin 7), fil bleu (A1 → pin 3). **6 points de soudure** (3 côté Feather + 3 côté capteur).

**Total soudure : environ 90 à 95 points.** Aucun composant CMS (montage en surface), tout est du traversant (through-hole), c'est-à-dire le type de soudure le plus simple. Si tu n'as jamais soudé, regarde 2–3 tutoriels YouTube sur la soudure through-hole avant de commencer, ça s'apprend en 30 minutes.

---

## 4. Schéma de câblage

Le câblage est très simple : seulement **3 fils** à souder pour le capteur ultrasonique. Les FeatherWings (OLED et RTC) se connectent automatiquement via les stacking headers (bus I2C).

### Connexions du capteur MB1040

```
MaxBotix MB1040          Adafruit Feather M0 Adalogger
┌─────────────┐          ┌──────────────────────────┐
│             │          │                          │
│  Pin 6 (5V)├──────────┤ 3V3  (alimentation 3.3V) │  ← Fil ROUGE
│             │          │                          │
│  Pin 7 (GND)├─────────┤ GND  (masse)             │  ← Fil NOIR
│             │          │                          │
│  Pin 3 (AN)├──────────┤ A1   (entrée analogique) │  ← Fil BLEU
│             │          │                          │
└─────────────┘          └──────────────────────────┘
```

### Pinout du capteur MB1040 (vue de face)

```
        ┌─────────┐
        │  (( ))  │  ← Transducteur ultrasonique
        │         │
        └─┬┬┬┬┬┬┬┘
          │││││││
          1234567
          
Pin 1 : BW  (non connecté)
Pin 2 : PW  (non connecté)
Pin 3 : AN  (sortie analogique) → A1 du Feather
Pin 4 : RX  (non connecté)
Pin 5 : TX  (non connecté)
Pin 6 : +5V (alimentation)     → 3V3 du Feather
Pin 7 : GND (masse)            → GND du Feather
```

### Architecture d'empilement

```
        ┌───────────────────────┐
        │   FeatherWing OLED    │  ← Couche 3 (dessus, écran visible)
        │   (écran 128×32)      │
        ├───────────────────────┤
        │   FeatherWing RTC     │  ← Couche 2 (milieu)
        │   (horloge DS3231)    │
        ├───────────────────────┤
        │   Feather M0          │  ← Couche 1 (base)
        │   Adalogger           │
        │   [SD] [USB] [Batt]   │
        └───────────────────────┘
              │ │ │
              3 fils vers MB1040
              (capteur pointe vers le bas, vers l'eau)
```

Les stacking headers permettent d'empiler les 3 cartes. Les pins I2C (SDA/SCL) sont partagées automatiquement entre le Feather, l'OLED et la RTC. Les 3 fils du capteur peuvent être soudés sur n'importe quel niveau de l'empilement car les pins sont partagées.

---

## 5. Installation logicielle

### Étape 1 : Installer Arduino IDE

Télécharger depuis https://www.arduino.cc/en/software (Windows, macOS, ou Linux).

### Étape 2 : Ajouter le support Adafruit SAMD (Feather M0)

1. Ouvrir Arduino IDE → **Fichier** → **Préférences**
2. Dans « URL de gestionnaire de cartes supplémentaires », ajouter :
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```
3. Aller dans **Outils** → **Type de carte** → **Gestionnaire de cartes**
4. Chercher « Adafruit SAMD » et installer
5. Sélectionner la carte : **Outils** → **Type de carte** → **Adafruit Feather M0**

### Étape 3 : Installer les bibliothèques

Aller dans **Outils** → **Gérer les bibliothèques** et installer :

| Bibliothèque | Auteur | Utilité |
|--------------|--------|---------|
| RTClib | Adafruit | Communication avec l'horloge DS3231 |
| Adafruit GFX Library | Adafruit | Fonctions graphiques de base pour l'OLED |
| Adafruit SSD1306 | Adafruit | Driver spécifique de l'écran OLED |
| SD | Arduino (incluse) | Lecture/écriture sur carte microSD |
| SPI | Arduino (incluse) | Communication SPI (carte SD) |
| Wire | Arduino (incluse) | Communication I2C (OLED + RTC) |

---

## 6. Ordre d'utilisation des programmes

### Programme 1 : `reglage_rtc.ino`

**Quand** : Une seule fois, avant tout le reste.

**Pourquoi** : Mettre l'horloge RTC à l'heure. Elle conservera l'heure même sans alimentation grâce à sa pile bouton intégrée.

**Comment** :
1. Brancher le Feather en USB
2. Ouvrir `reglage_rtc.ino` dans Arduino IDE
3. Compiler et uploader
4. Ouvrir le Serial Monitor (115200 baud)
5. Vérifier que l'heure affichée est correcte

### Programme 2 : `calibration.ino`

**Quand** : Après l'assemblage complet du capteur dans son boîtier.

**Pourquoi** : Obtenir la courbe de calibration propre à ton capteur (gain et offset pour la régression linéaire).

**Comment** :
1. Uploader `calibration.ino`
2. Ouvrir le Serial Monitor (115200 baud)
3. Placer le capteur face à un mur plat à chaque distance demandée
4. Appuyer sur Entrée pour chaque mesure
5. Noter les valeurs CSV affichées
6. Faire la régression linéaire (dans Python ou un tableur)

### Programme 3 : `maregraphe_firmware.ino`

**Quand** : Pour le déploiement terrain.

**Pourquoi** : C'est le firmware principal qui mesure et enregistre les données.

**Comment** :
1. Uploader `maregraphe_firmware.ino`
2. Insérer la carte MicroSD
3. Brancher la batterie
4. Déployer le capteur au-dessus de l'eau
5. Récupérer la carte SD après la période de mesure

### Programme 4 : `traitement_donnees.py`

**Quand** : Après récupération des données.

**Pourquoi** : Analyser les données, comparer avec SHOM, faire l'analyse spectrale.

**Comment** :
1. Copier `MAREE.CSV` de la carte SD dans le même dossier que le script
2. Télécharger les données SHOM correspondantes depuis https://refmar.shom.fr
3. Les renommer en `donnees_shom.csv`
4. Exécuter : `python3 traitement_donnees.py`
5. Le script peut aussi tourner en mode démo (sans données réelles) pour tester

---

## 7. Conseils de déploiement

### Choix du site

Le capteur doit pointer **verticalement vers le bas**, vers la surface de l'eau, à une distance comprise entre 15 et 645 cm (plage utile du MB1040). Un quai, une jetée, ou un pont dans un port sont des emplacements idéaux. Privilégier un endroit abrité (moins de vagues = mesures plus propres). Idéalement, déployer à proximité d'un marégraphe REFMAR du SHOM pour pouvoir comparer tes mesures avec les données officielles.

### Montage du boîtier

D'après l'article, un simple bocal en plastique transparent fonctionne très bien :
1. Percer un trou dans le fond du bocal pour laisser passer les ultrasons du capteur
2. Fixer un gobelet plastique retourné au-dessus du trou pour protéger de la pluie
3. Placer l'électronique à l'intérieur du bocal, capteur orienté vers le bas
4. Fermer hermétiquement le bocal (joint + silicone si nécessaire)
5. Fixer solidement le tout au quai avec des colliers ou un support imprimé 3D

### Autonomie

Avec la batterie LiPo 6600 mAh et une consommation de ~5.5 mA en moyenne, l'autonomie théorique est d'environ **50 jours**. Avec une batterie 10 000 mAh (comme dans l'article), on atteint **76 jours**. Pour un déploiement de 2–3 semaines (suffisant pour un TIPE), même une batterie de 3000 mAh suffirait (~23 jours).

---

## 8. Pour le TIPE : ce que tu peux présenter

1. **Théorie** : Explication de la mécanique des marées (forces gravitationnelles dans un référentiel géocentrique non galiléen, double bourrelet océanique, composantes harmoniques M2, S2, K1, O1).
2. **Principe de mesure** : fonctionnement du capteur ultrasonique (émission/réception d'ondes à 42 kHz, calcul de distance par temps de vol, analogie avec le sonar).
3. **Construction** : photos de l'assemblage, schéma de câblage, choix techniques.
4. **Calibration** : graphique ADC vs distance réelle, régression linéaire, R².
5. **Résultats terrain** : courbe de marée mesurée, comparaison avec SHOM/REFMAR.
6. **Analyse spectrale (FFT)** : identification des composantes harmoniques.
7. **Discussion** : RMSE, sources d'erreur, effets météo locaux, limites du modèle.

---

## Références

- Bresnahan, P., et al. (2023). A low-cost, DIY ultrasonic water level sensor for education, citizen science, and research. *Oceanography*, 36(1):51–58.
- SHOM — Réseau REFMAR : https://refmar.shom.fr
- GitHub du projet original : https://github.com/SUPScientist/Seaport_Tide-SLR
