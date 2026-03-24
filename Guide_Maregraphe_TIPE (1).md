# Marégraphe DIY — Guide complet pour le TIPE

## Contexte

Ce guide accompagne la réalisation d'un marégraphe (capteur de niveau d'eau) à ultrasons basé sur l'article de Bresnahan et al. (2023) publié dans *Oceanography*. Le circuit reproduit fidèlement la version « North Carolina » de l'article, utilisant un Adafruit Feather M0 Adalogger comme microcontrôleur principal.

---

## 1. Liste de matériel à acheter

### Composants électroniques principaux

| Composant | Rôle | Où acheter | Prix approximatif |
|-----------|------|------------|-------------------|
| **Adafruit Feather M0 Adalogger** | Microcontrôleur + lecteur SD intégré | Mouser.fr, Adafruit.com, GoTronic.fr | ~22 € |
| **MaxBotix MB1040 LV-MaxSonar-EZ4** | Capteur de distance ultrasonique | Mouser.fr, Adafruit.com, Robotshop.com | ~30 € |
| **Adafruit FeatherWing OLED 128×32** | Écran d'affichage temps réel | Mouser.fr, Adafruit.com, GoTronic.fr | ~16 € |
| **Adafruit FeatherWing RTC (DS3231)** | Horloge temps réel | Mouser.fr, Adafruit.com, GoTronic.fr | ~10 € |
| **Carte MicroSD** (8 Go suffisent) | Stockage des données | Amazon.fr, n'importe quel revendeur | ~5 € |
| **Batterie LiPo 3.7V 6600 mAh** (connecteur JST) | Alimentation autonome | Adafruit.com, Amazon.fr | ~25 € |

### Consommables et connectique

| Composant | Rôle | Prix approximatif |
|-----------|------|-------------------|
| **Stacking headers pour Feather** (×3 jeux) | Empiler les FeatherWings | ~4 € |
| **Breadboard demi-taille** | Prototypage (optionnel si soudure directe) | ~5 € |
| **Fil rigide 22 AWG** (noir, rouge, bleu) | 3 connexions vers le capteur | ~3 € le lot |
| **Soudure sans plomb** | Assemblage | ~10 € |
| **Câble USB-A vers Micro-USB** | Programmation du Feather | ~3 € |

### Outils nécessaires (non consommables)

- Fer à souder (température réglable recommandé)
- Pince coupante / pince à dénuder
- Lecteur de carte MicroSD (pour récupérer les données)
- Ordinateur avec Arduino IDE installé

### Pour le boîtier (déploiement terrain)

| Composant | Rôle | Prix approximatif |
|-----------|------|-------------------|
| **Boîte plastique transparente hermétique** (type bocal à glace ou boîte alimentaire) | Protection des composants | ~5 € (récup) |
| **Gobelet plastique** | Protection pluie pour le capteur | ~0 € (récup) |
| **Colliers de serrage / serre-câbles** | Fixation sur un quai ou une rambarde | ~3 € |
| **Silicone ou colle chaude** | Étanchéité autour du capteur | ~5 € |

### Budget total estimé : **~120–150 €**

> **Conseil d'achat** : Mouser.fr et GoTronic.fr sont les meilleurs distributeurs en France pour les composants Adafruit. Les délais sont généralement de 3 à 7 jours. Si tu es pressé, vérifie aussi Amazon.fr pour la batterie et la carte SD.

---

## 2. Schéma de câblage

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
        │   FeatherWing OLED    │  ← Couche 3 (dessus)
        │   (écran 128×32)      │
        ├───────────────────────┤
        │   FeatherWing RTC     │  ← Couche 2
        │   (horloge DS3231)    │
        ├───────────────────────┤
        │   Feather M0          │  ← Couche 1 (base)
        │   Adalogger           │
        │   [SD] [USB] [Batt]   │
        └───────────────────────┘
              │ │ │
              3 fils vers MB1040
```

Les stacking headers permettent d'empiler les 3 cartes. Les pins I2C (SDA/SCL) sont partagées automatiquement entre le Feather, l'OLED et la RTC.

---

## 3. Installation logicielle

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

- `RTClib` (par Adafruit)
- `Adafruit GFX Library`
- `Adafruit SSD1306`
- `SD` (normalement déjà incluse)
- `SPI` (normalement déjà incluse)
- `Wire` (normalement déjà incluse)

---

## 4. Ordre d'utilisation des programmes

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

## 5. Conseils de déploiement

### Choix du site

- Le capteur doit pointer **verticalement vers le bas**, vers la surface de l'eau
- Distance capteur–eau : entre 15 cm et 645 cm (plage du MB1040)
- Idéalement proche d'un marégraphe REFMAR pour comparaison
- Un quai, une jetée ou un pont sont des emplacements idéaux
- Éviter les zones avec beaucoup de vagues (le moyennage aide, mais un endroit abrité est préférable)

### Montage du boîtier

D'après l'article, un simple bocal en plastique transparent fonctionne très bien :
1. Percer un trou dans le fond du bocal pour laisser passer les ultrasons du capteur
2. Fixer un gobelet plastique retourné au-dessus du trou pour protéger de la pluie
3. Placer l'électronique à l'intérieur du bocal, capteur orienté vers le bas
4. Fermer hermétiquement le bocal (joint + silicone si nécessaire)
5. Fixer solidement le tout au quai avec des colliers ou un support imprimé 3D

### Autonomie

Avec la batterie LiPo 6600 mAh et une consommation de ~5.5 mA en moyenne, l'autonomie théorique est d'environ **50 jours**. Avec une batterie 10 000 mAh (comme dans l'article), on atteint **76 jours**.

---

## 6. Pour le TIPE : ce que tu peux présenter

1. **Théorie** : Explication de la mécanique des marées (forces gravitationnelles, référentiel non galiléen, composantes harmoniques M2, S2, K1...)
2. **Construction** : Photos de l'assemblage, schéma de câblage, explication du principe ultrasonique
3. **Calibration** : Graphique calibration (comme Figure 3 de l'article), régression linéaire, R²
4. **Résultats terrain** : Courbe de marée mesurée, comparaison avec SHOM/REFMAR
5. **Analyse spectrale** : FFT montrant les composantes harmoniques identifiées (M2, S2...)
6. **Discussion** : RMSE, sources d'erreur, limites du modèle, effets météo locaux

---

## Références

- Bresnahan, P., et al. (2023). A low-cost, DIY ultrasonic water level sensor for education, citizen science, and research. *Oceanography*, 36(1):51–58.
- SHOM — Réseau REFMAR : https://refmar.shom.fr
- GitHub du projet original : https://github.com/SUPScientist/Seaport_Tide-SLR
