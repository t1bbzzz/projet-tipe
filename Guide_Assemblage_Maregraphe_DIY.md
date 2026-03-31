# Guide d'assemblage pas-à-pas du marégraphe DIY

## D'après Bresnahan et al. (2023) — Version North Carolina (Feather M0 Adalogger)

> Ce guide est écrit pour quelqu'un qui n'a jamais touché à un Arduino. Chaque étape est détaillée dans l'ordre chronologique exact. Ne saute aucune étape, lis tout avant de commencer.

---

## PHASE 0 — Avant de commencer

### 0.1. Vérifie que tu as tout le matériel

Pose tout sur une table propre et bien éclairée. Coche chaque élément :

- [ ] Adafruit Feather M0 Adalogger
- [ ] MaxBotix MB1040 LV-MaxSonar-EZ4
- [ ] Adafruit FeatherWing OLED 128×32
- [ ] Adafruit FeatherWing RTC (DS3231)
- [ ] Stacking headers pour Feather (3 jeux, chaque jeu = 1 barrette 16 pins + 1 barrette 12 pins)
- [ ] Carte MicroSD (8 Go minimum)
- [ ] Batterie LiPo 3.7V avec connecteur JST-PH 2 broches
- [ ] Fil rigide 22 AWG (3 couleurs : rouge, noir, bleu)
- [ ] Soudure sans plomb
- [ ] Câble USB-A vers Micro-USB (câble de données, pas juste charge)
- [ ] Breadboard demi-taille (optionnel mais très recommandé)

### 0.2. Vérifie que tu as les outils

- [ ] Fer à souder (idéalement avec température réglable, 300–350 °C)
- [ ] Support pour fer à souder (le poser sur la table = danger)
- [ ] Éponge humide ou nettoyeur en laiton (pour nettoyer la panne du fer)
- [ ] Pince coupante
- [ ] Pince à dénuder (ou un cutter, mais attention)
- [ ] Troisième main / pince de maintien (très recommandé)
- [ ] Lunettes de protection (la soudure peut projeter)
- [ ] Ordinateur avec port USB

### 0.3. Installe le logiciel AVANT de souder

C'est important de faire ça maintenant : si ton ordinateur ne reconnaît pas la carte, mieux vaut le savoir avant d'avoir tout soudé.

**Étape 1 : Installer Arduino IDE**
1. Va sur https://www.arduino.cc/en/software
2. Télécharge la version pour ton système (Windows / macOS / Linux)
3. Installe normalement

**Étape 2 : Ajouter le support du Feather M0**
1. Ouvre Arduino IDE
2. Va dans **Fichier → Préférences**
3. Dans le champ « URL de gestionnaire de cartes supplémentaires », colle cette URL :
   ```
   https://adafruit.github.io/arduino-board-index/package_adafruit_index.json
   ```
4. Clique OK
5. Va dans **Outils → Type de carte → Gestionnaire de cartes**
6. Dans la barre de recherche, tape « Adafruit SAMD »
7. Installe le paquet **Adafruit SAMD Boards**
8. Ferme le gestionnaire

**Étape 3 : Installer les bibliothèques**
1. Va dans **Outils → Gérer les bibliothèques**
2. Cherche et installe chacune de ces bibliothèques :
   - **RTClib** (par Adafruit) — pour l'horloge
   - **Adafruit GFX Library** (par Adafruit) — pour les graphiques OLED
   - **Adafruit SSD1306** (par Adafruit) — pour le driver de l'écran OLED
3. Les bibliothèques SD, SPI et Wire sont déjà incluses dans Arduino IDE

**Étape 4 : Tester la connexion USB**
1. Branche le Feather M0 Adalogger à ton PC avec le câble USB (sans rien soudé, c'est normal)
2. Va dans **Outils → Type de carte** et sélectionne **Adafruit Feather M0**
3. Va dans **Outils → Port** et sélectionne le port qui apparaît (ex : COM3 sur Windows, /dev/ttyACM0 sur Linux)
4. Si aucun port n'apparaît : essaie un autre câble USB (certains sont charge-only), ou appuie rapidement 2× sur le bouton Reset du Feather
5. Si un port apparaît : bravo, ton PC reconnaît la carte. Débranche-la pour passer à la soudure.

---

## PHASE 1 — Soudure des stacking headers

> C'est l'étape la plus longue. Environ 90 points de soudure au total. Prends ton temps. La qualité de tes soudures détermine la fiabilité de tout le projet.

### Rappel soudure pour débutant

Si tu n'as jamais soudé, regarde cette vidéo de 5 min avant de commencer (ou une équivalente) : cherche « how to solder through hole » sur YouTube.

Les principes clés :
- **Chauffe le pad ET la pin ensemble** avec la pointe du fer (2–3 secondes)
- **Amène la soudure sur le joint** (pas sur le fer), elle doit fondre au contact du pad chaud
- **Retire la soudure** puis **retire le fer**
- Un bon joint de soudure a la forme d'un petit cône brillant
- Un mauvais joint (« cold joint ») est terne et granuleux → refais-le

### 1.1. Souder les stacking headers sur le Feather M0 Adalogger

C'est la carte la plus importante, elle sera en bas de l'empilement.

**Matériel nécessaire :** 1 jeu de stacking headers (1 barrette de 16 pins + 1 barrette de 12 pins)

**Procédure :**

1. **Prépare la breadboard comme support** : insère les stacking headers dans la breadboard, **pins longues (mâles) vers le bas**, dans la breadboard. Les trous femelles doivent pointer vers le haut. La breadboard maintient les headers parfaitement droits pendant la soudure.

2. **Pose le Feather M0 par-dessus** : les trous du Feather doivent s'enfiler sur les pins courtes qui dépassent vers le haut. Le composant USB du Feather doit être accessible (vers l'extérieur).

3. **Vérifie l'alignement** : le Feather doit être bien à plat, parallèle à la breadboard. Ajuste si nécessaire.

4. **Soude une pin à chaque extrémité** d'abord (une pin sur la barrette 16 et une sur la barrette 12). Cela fixe les headers en place. Vérifie à nouveau l'alignement — c'est ta dernière chance de corriger.

5. **Soude toutes les autres pins** : une par une, en prenant ton temps. 32 points de soudure au total (16 + 12 de chaque côté, mais en fait c'est 16 d'un côté et 12 de l'autre = 28 points).

6. **Vérifie visuellement** : retourne la carte et regarde chaque joint. Chaque pin doit avoir un petit cône de soudure propre. Aucune pin ne doit être en court-circuit avec sa voisine (pas de pont de soudure).

7. **Retire délicatement le Feather de la breadboard** en tirant bien droit.

### 1.2. Souder les stacking headers sur la FeatherWing RTC

La RTC sera au milieu de l'empilement, donc elle a besoin de stacking headers (pas de headers simples).

**Matériel nécessaire :** 1 jeu de stacking headers

**Procédure :** Exactement la même que pour le Feather M0 (étape 1.1). La FeatherWing RTC a le même format (Feather form factor). Environ 28 points de soudure.

**Note importante :** La FeatherWing RTC a aussi un slot pour une pile bouton CR1220. Si la pile n'est pas déjà en place, insère-la maintenant (face + vers le haut). Cette pile permet à l'horloge de garder l'heure même sans batterie principale.

### 1.3. Souder les headers sur la FeatherWing OLED

L'OLED sera sur le dessus de l'empilement. Tu peux utiliser des **stacking headers** (si tu veux pouvoir empiler autre chose dessus plus tard) ou des **headers mâles simples** (moins chers, suffisants).

**Matériel nécessaire :** 1 jeu de stacking headers OU 1 jeu de headers mâles simples

**Procédure :** Même méthode que les étapes précédentes. Environ 28 points de soudure.

### 1.4. Test d'empilement à sec

Avant de continuer, empile les trois cartes pour vérifier que tout s'emboîte correctement :

```
        ┌───────────────────────┐
        │   FeatherWing OLED    │  ← Dessus (écran visible)
        ├───────────────────────┤
        │   FeatherWing RTC     │  ← Milieu
        ├───────────────────────┤
        │   Feather M0          │  ← Base
        │   Adalogger           │
        └───────────────────────┘
```

- Les trois cartes doivent s'emboîter sans forcer
- L'écran OLED doit être visible sur le dessus
- Le port USB et le connecteur batterie du Feather doivent être accessibles en bas
- Le slot MicroSD du Feather doit être accessible

Si tout s'emboîte bien, **sépare les cartes** pour l'étape suivante.

---

## PHASE 2 — Câblage du capteur ultrasonique

> Seulement 3 fils et 6 points de soudure. C'est la partie la plus critique électriquement.

### 2.1. Prépare les 3 fils

Coupe 3 morceaux de fil rigide 22 AWG d'environ **10 à 15 cm** chacun :
- **1 fil ROUGE** → alimentation
- **1 fil NOIR** → masse
- **1 fil BLEU** → signal analogique

Dénude environ 5 mm de gaine à chaque extrémité de chaque fil (avec la pince à dénuder ou un cutter, délicatement).

### 2.2. Identifie les pins du capteur MB1040

Regarde le capteur de face (le cylindre du transducteur ultrasonique face à toi). Les 7 pins sont en bas. De gauche à droite :

```
Pin 1 : BW  — Non utilisé
Pin 2 : PW  — Non utilisé
Pin 3 : AN  — Sortie analogique (SIGNAL)  ← FIL BLEU
Pin 4 : RX  — Non utilisé
Pin 5 : TX  — Non utilisé
Pin 6 : +5V — Alimentation               ← FIL ROUGE
Pin 7 : GND — Masse                       ← FIL NOIR
```

⚠️ **Attention à ne pas confondre les pins !** Compte bien de gauche à droite en regardant le capteur de face. La datasheet du MB1040 est disponible sur le site de MaxBotix si tu as un doute.

### 2.3. Identifie les pins du Feather M0

Sur le Feather M0 Adalogger (ou sur l'un des FeatherWings empilés, puisque les pins sont partagées), tu dois trouver :
- **3V3** (ou 3.3V) — alimentation 3.3 volts
- **GND** — masse (il y en a plusieurs, n'importe lequel convient)
- **A1** — entrée analogique numéro 1

Ces labels sont sérigraphiés (imprimés en blanc) directement sur la carte. Regarde attentivement les deux côtés.

### 2.4. Soude les 3 fils

**Côté capteur MB1040 :**
1. Soude le fil ROUGE sur la **pin 6** (+5V) du capteur
2. Soude le fil NOIR sur la **pin 7** (GND) du capteur
3. Soude le fil BLEU sur la **pin 3** (AN) du capteur

**Astuce :** Utilise une troisième main ou du ruban adhésif pour maintenir le capteur immobile pendant la soudure.

**Côté Feather / FeatherWing :**

Tu as deux options pour souder les fils côté Feather :

- **Option A (recommandée par l'article)** : Souder directement dans les trous de prototypage (thru holes) disponibles sur les FeatherWings. Cela donne un assemblage plus compact et robuste.
- **Option B (plus simple pour un débutant)** : Souder les fils sur les pins des stacking headers du Feather lui-même (les pins 3V3, GND et A1). C'est plus facile mais moins propre.

Dans les deux cas :
1. Soude le fil ROUGE sur la pin **3V3**
2. Soude le fil NOIR sur la pin **GND**
3. Soude le fil BLEU sur la pin **A1**

### 2.5. Vérification du câblage

**C'est l'étape la plus importante.** Une erreur de câblage peut endommager le capteur ou le Feather.

Vérifie visuellement :
- Fil ROUGE : pin 6 du MB1040 → 3V3 du Feather ✓
- Fil NOIR : pin 7 du MB1040 → GND du Feather ✓
- Fil BLEU : pin 3 du MB1040 → A1 du Feather ✓
- Aucun fil ne touche un autre fil ou une autre pin ✓
- Aucun pont de soudure (soudure qui déborde et connecte deux pins voisines) ✓

---

## PHASE 3 — Premier test électronique

> On va vérifier que chaque composant fonctionne AVANT de tout assembler dans le boîtier.

### 3.1. Empile les cartes

Empile les trois cartes dans l'ordre (Feather en bas, RTC au milieu, OLED en haut). Le capteur MB1040 est relié par ses 3 fils et pend à l'extérieur de l'empilement, c'est normal.

### 3.2. Insère la carte MicroSD

Insère une carte MicroSD formatée en FAT32 dans le slot du Feather M0 (accessible sur le côté, sous l'empilement). Pousse-la jusqu'au clic.

### 3.3. Branche en USB et uploade le sketch de réglage RTC

1. Branche le Feather au PC via USB
2. Dans Arduino IDE :
   - **Outils → Type de carte → Adafruit Feather M0**
   - **Outils → Port → [ton port]**
3. Ouvre le fichier `reglage_rtc.ino` (que je t'ai fourni précédemment)
4. Clique sur le bouton **Upload** (flèche vers la droite, en haut à gauche)
5. Attends que ça compile et uploade (ça prend 30 secondes à 1 minute)
6. Ouvre le **Serial Monitor** : **Outils → Moniteur série** (ou Ctrl+Maj+M)
7. Mets le baudrate à **115200** (en bas à droite du moniteur)
8. Tu dois voir l'heure s'afficher et se mettre à jour chaque seconde

**Si tu vois l'heure qui défile :** la RTC fonctionne ✓

**Si rien ne s'affiche :**
- Vérifie que le baudrate est à 115200
- Appuie sur le bouton Reset du Feather
- Si toujours rien : vérifie les soudures de la FeatherWing RTC

### 3.4. Teste l'écran OLED

Uploade le fichier `calibration.ino`. Si l'écran OLED affiche du texte (« Point 1/8, Distance: 30 cm... »), l'écran fonctionne ✓.

### 3.5. Teste le capteur MB1040

Toujours avec `calibration.ino` :
1. Tiens le capteur face à un mur, à environ 30 cm
2. Appuie sur Entrée dans le Serial Monitor
3. Tu dois voir des valeurs ADC s'afficher (nombres entre 30 et 500 environ)
4. Éloigne le capteur du mur → les valeurs doivent augmenter
5. Rapproche-le → les valeurs doivent diminuer

**Si les valeurs changent avec la distance :** le capteur fonctionne ✓

**Si les valeurs restent à 0 ou ne changent pas :**
- Vérifie le câblage (pins 3, 6, 7 du capteur)
- Vérifie qu'il n'y a pas de pont de soudure
- Vérifie que le fil bleu est bien sur A1 (pas A0 ni A2)

### 3.6. Teste la carte SD

Uploade le firmware principal `maregraphe_firmware.ino`. Attends une mesure (6 min par défaut, tu peux réduire temporairement à 10 secondes pour le test en changeant `SAMPLE_INTERVAL_MIN` à `0` et le délai dans le loop à `10000`). Puis éteins le Feather, retire la carte SD, et vérifie sur ton PC qu'un fichier `MAREE.CSV` a été créé avec des données.

**Si le fichier existe et contient des données :** la SD fonctionne ✓

---

## PHASE 4 — Calibration

> La calibration est indispensable. Ne saute pas cette étape. C'est elle qui transforme ton capteur à 5 cm de précision en capteur à ~1.5 cm de précision.

### 4.1. Protocole de calibration

D'après l'article de Bresnahan et al., la calibration se fait **une fois le capteur entièrement assemblé dans son boîtier** (car le boîtier peut interférer avec les mesures). Mais tu peux faire une première calibration « à nu » pour vérifier que tout marche, puis refaire une calibration finale après mise en boîtier.

**Matériel :** un mètre ruban ou un mètre rigide, un mur plat ou un sol plat.

**Procédure :**

1. Uploade `calibration.ino` sur le Feather
2. Ouvre le Serial Monitor (115200 baud)
3. Pour chaque distance demandée par le programme (30, 60, 100, 150, 200, 254, 381, 508 cm) :
   - Place le capteur face au mur à la distance indiquée (mesurée avec le mètre)
   - Maintiens-le bien stable et perpendiculaire au mur
   - Appuie sur Entrée dans le Serial Monitor
   - Le programme prend 50 mesures et affiche la moyenne
4. Note les paires de valeurs (distance_réelle, ADC_moyen) affichées en format CSV

### 4.2. Régression linéaire

Avec les paires de valeurs, fais une régression linéaire. Tu peux utiliser :
- Un tableur (Excel, LibreOffice Calc) avec la fonction DROITEREG ou un graphique XY avec courbe de tendance linéaire
- Python avec numpy : `np.polyfit(adc_vals, distances_cm, 1)`

Tu obtiens : **distance_cm = gain × ADC + offset**

Note le **gain** (pente) et l'**offset** (ordonnée à l'origine). Tu en auras besoin pour le traitement des données.

Dans l'article, les auteurs ont obtenu un gain de 1.28 cm/count et un offset de −1.48 cm, avec un R² de 0.9999.

---

## PHASE 5 — Assemblage du boîtier

> Le boîtier protège l'électronique pendant le déploiement terrain. L'article de Bresnahan et al. utilise un simple bocal en plastique recyclé.

### 5.1. Matériaux pour le boîtier

- Un bocal ou boîte en plastique transparent hermétique (type pot de glace, bocal alimentaire, boîte Tupperware)
- Un gobelet en plastique (protection pluie)
- Silicone ou colle chaude
- Colliers de serrage / serre-câbles
- Éventuellement un support imprimé 3D ou une équerre pour la fixation

### 5.2. Préparation du bocal

1. **Perce un trou** dans le fond du bocal (ou dans le couvercle, selon comment tu montes le capteur). Le trou doit être juste assez grand pour que le cylindre du transducteur ultrasonique du MB1040 puisse y passer ou y être exposé. Le transducteur doit avoir une vue dégagée vers le bas (vers l'eau).

2. **Positionne le capteur MB1040** de façon que le transducteur soit aligné avec le trou, pointant vers l'extérieur (vers le bas quand le boîtier sera installé au-dessus de l'eau).

3. **Scelle autour du capteur** avec du silicone ou de la colle chaude pour empêcher l'eau de pluie d'entrer.

4. **Fixe un gobelet plastique retourné** au-dessus du trou (à l'extérieur du bocal) pour protéger le capteur de la pluie directe. L'article note que cette solution simple a survécu à la tempête tropicale Colin sans interruption.

5. **Place l'électronique** (l'empilement Feather + RTC + OLED) et la batterie à l'intérieur du bocal. Assure-toi que rien ne bouge en calant avec de la mousse ou du papier bulle.

6. **Ferme hermétiquement** le bocal.

### 5.3. Refais la calibration

Maintenant que tout est dans le boîtier, refais la calibration (Phase 4) avec le capteur dans sa configuration finale. Le boîtier peut légèrement modifier les mesures.

---

## PHASE 6 — Upload du firmware final et déploiement

### 6.1. Upload du firmware principal

1. Ouvre `maregraphe_firmware.ino` dans Arduino IDE
2. Si tu veux modifier l'intervalle de mesure, change la ligne :
   ```cpp
   #define SAMPLE_INTERVAL_MIN  6    // 6 minutes entre chaque mesure
   ```
3. Uploade le firmware sur le Feather
4. Ouvre le Serial Monitor et vérifie qu'une première mesure s'affiche correctement (attends 6 min ou modifie temporairement l'intervalle)
5. Vérifie que l'OLED affiche la distance et l'heure

### 6.2. Préparation au déploiement

1. Formate la carte MicroSD en FAT32
2. Insère-la dans le Feather
3. Branche la batterie LiPo (connecteur JST)
4. Vérifie sur l'OLED que les mesures s'affichent
5. Débranche le câble USB (le Feather fonctionne maintenant sur batterie)
6. Ferme le boîtier

### 6.3. Choix du site de déploiement

D'après l'article et le README du GitHub :
- Le capteur doit pointer **verticalement vers le bas**, vers la surface de l'eau
- La distance capteur–eau doit rester entre **15 cm et 645 cm** (plage du MB1040) à tout moment (y compris à marée haute et à marée basse)
- Idéalement, déploie à proximité d'un marégraphe REFMAR/SHOM pour comparaison
- Un quai, une jetée, ou un pont dans un port sont des emplacements idéaux
- L'endroit doit être abrité (moins de vagues = mesures plus propres)
- Fixe le boîtier solidement avec des colliers de serrage, vis, ou support 3D
- **Assure-toi que le capteur est bien de niveau** (perpendiculaire à la surface de l'eau, il pointe droit vers le bas)

### 6.4. Vérification post-installation

Une fois le capteur installé :
1. Regarde l'écran OLED à travers le bocal transparent — les mesures doivent s'afficher
2. Vérifie que la distance affichée semble cohérente avec la distance réelle capteur–eau
3. Note l'heure et la distance sur un carnet (tu en auras besoin pour la constante « a » plus tard)

---

## PHASE 7 — Récupération et traitement des données

### 7.1. Récupération

Après ta période de mesure (quelques jours à quelques semaines) :
1. Débranche la batterie
2. Retire la carte MicroSD
3. Insère-la dans un lecteur SD sur ton PC
4. Copie le fichier `MAREE.CSV` sur ton ordinateur

### 7.2. Traitement

Utilise le script `traitement_maregraphe_diy.py` que je t'ai fourni. Il fait exactement les mêmes graphiques que ton `TIPE.py` mais à partir de tes données de carte SD.

### 7.3. Conversion distance → hauteur d'eau

La formule est simple (Équation 1 de l'article) :

**H = a − x**

- H = hauteur d'eau
- a = position fixe du capteur (constante à déterminer)
- x = distance mesurée par le capteur

Pour déterminer « a », soit tu fais un relevé topographique (méthode rigoureuse), soit tu ajustes « a » par essai-erreur jusqu'à ce que ta courbe se superpose aux données SHOM/REFMAR (méthode de l'article).

---

## Résumé chronologique : l'ordre de tout le projet

1. Commander le matériel
2. Installer Arduino IDE + bibliothèques (Phase 0)
3. Tester la connexion USB du Feather nu (Phase 0)
4. Souder les stacking headers sur les 3 cartes (Phase 1)
5. Souder les 3 fils entre le capteur et le Feather (Phase 2)
6. Empiler les cartes et tester chaque composant (Phase 3)
7. Calibrer le capteur (Phase 4)
8. Assembler le boîtier (Phase 5)
9. Recalibrer dans le boîtier (Phase 5)
10. Uploader le firmware final (Phase 6)
11. Déployer sur le terrain (Phase 6)
12. Récupérer les données et analyser (Phase 7)

---

## En cas de problème

| Problème | Cause probable | Solution |
|----------|----------------|----------|
| L'Arduino IDE ne voit pas le Feather | Câble USB charge-only | Essaye un autre câble. Appuie 2× rapidement sur Reset. |
| Erreur de compilation | Bibliothèque manquante | Vérifie que RTClib, Adafruit GFX et SSD1306 sont installées. |
| L'OLED n'affiche rien | Headers mal soudés | Vérifie les soudures. Vérifie l'adresse I2C (0x3C par défaut). |
| La RTC donne la mauvaise heure | RTC pas réglée | Re-uploade reglage_rtc.ino (l'heure = heure de compilation). |
| Le capteur donne toujours 0 | Mauvais câblage | Vérifie que le fil bleu est sur A1 et que le fil rouge est sur 3V3. |
| La SD ne fonctionne pas | Carte pas en FAT32 | Reformate en FAT32. Vérifie que le pin CS est bien 4 (défaut Feather). |
| Valeurs aberrantes ponctuelles | Objet flottant sous le capteur | Normal, le script Python filtre les outliers. |
| Autonomie trop courte | Batterie trop petite | Utilise une batterie plus grosse ou réduis la fréquence de mesure. |

---

## Références

- Bresnahan, P., et al. (2023). A low-cost, DIY ultrasonic water level sensor for education, citizen science, and research. *Oceanography*, 36(1):51–58. https://doi.org/10.5670/oceanog.2023.101
- GitHub du projet original : https://github.com/SUPScientist/Seaport_Tide-SLR
- Version mise à jour (COAST Lab) : https://github.com/COAST-Lab/Open-Water-Level
- Tutoriel soudure Adafruit : https://learn.adafruit.com/adafruit-guide-excellent-soldering
- MaxBotix MB1040 datasheet : https://www.maxbotix.com/ultrasonic_sensors/mb1040.htm
- Tutoriel Arduino × MaxBotix : https://www.maxbotix.com/Arduino-Ultrasonic-Sensors-085/
- SHOM / REFMAR : https://refmar.shom.fr
