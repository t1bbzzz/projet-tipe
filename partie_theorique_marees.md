# Partie Théorique — Les Marées : Origines Physiques

**TIPE — MP/PC\* | Auteur : Tibbs**

---

## I. Cadre d'étude et référentiels

### 1.1 Le problème physique

La marée est la variation périodique du niveau des océans résultant de l'action gravitationnelle de la **Lune** et du **Soleil** sur les masses d'eau terrestres. Pour comprendre pourquoi deux marées hautes et deux marées basses se produisent par jour, il ne suffit pas de considérer la Terre comme un point matériel soumis à l'attraction lunaire : il faut tenir compte de son **étendue spatiale** et du fait que le champ gravitationnel de la Lune **varie d'un point à l'autre** à sa surface.

On considère dans la suite un astre attracteur $A$ unique (Lune ou Soleil) de masse $m_A$, situé à une distance $D$ du centre $T$ de la Terre, de rayon $R_T$.

### 1.2 Analyse dans le référentiel de Copernic $\mathcal{R}_C$

Le référentiel de Copernic est supposé **galiléen**. Dans ce référentiel, tout point matériel $M$ de masse $m$ situé à la surface de la Terre est soumis à la force gravitationnelle exercée par $A$ :

$$\vec{F} = G\frac{m\,m_A}{MA^2}\,\vec{u}_{MA} = m\,\vec{\mathcal{G}}(M)$$

où $\vec{\mathcal{G}}(M)$ est le champ de gravitation de $A$ en $M$. Ce champ décroît en $1/MA^2$ et change de direction d'un point $M$ à un autre : les différents points de la Terre tendent donc à se déplacer de manière **distincte**, comme si la planète était étirée. C'est cette non-uniformité qui est à l'origine des marées.

---

## II. Le champ de marée

### 2.1 Passage au référentiel géocentrique $\mathcal{R}_G$

On se place dans le référentiel géocentrique $\mathcal{R}_G$, d'origine $T$, animé d'un mouvement de **translation** dans $\mathcal{R}_C$. Ce référentiel **n'est pas galiléen** : son centre $T$ est soumis à l'accélération :

$$\vec{a}(T) = \vec{\mathcal{G}}(T)$$

On doit donc introduire une **force d'inertie d'entraînement** pour tout point $M$ dans $\mathcal{R}_G$ :

$$\vec{F}_{ie} = -m\,\vec{\mathcal{G}}(T)$$

> **Remarque :** Il n'y a pas de force de Coriolis ici car $\mathcal{R}_G$ est en translation (pas en rotation) par rapport à $\mathcal{R}_C$.

La résultante des forces s'exerçant sur $M$ (hors forces d'origine terrestre $\vec{F}\,'$) s'écrit alors :

$$\vec{R} = \vec{F}\,' + m\,\vec{\mathcal{G}}(M) + \vec{F}_{ie} = \vec{F}\,' + m\!\left[\vec{\mathcal{G}}(M) - \vec{\mathcal{G}}(T)\right]$$

On définit le **champ de marée** $\vec{C}(M)$ et la **force de marée** $\vec{F}_m(M)$ par :

$$\boxed{\vec{C}(M) = \vec{\mathcal{G}}(M) - \vec{\mathcal{G}}(T)} \qquad \vec{F}_m(M) = m\,\vec{C}(M)$$

**L'idée fondamentale :** si le champ $\vec{\mathcal{G}}$ était uniforme, tous les points de la Terre auraient la même accélération, $\vec{C}$ serait nul, et les marées n'existeraient pas. **C'est la non-uniformité du champ gravitationnel** de la Lune et du Soleil à la surface de la Terre qui est la cause unique des marées — on parle d'**effet gravitationnel différentiel**.

### 2.2 Expression analytique — développement limité

On repère $M$ par ses coordonnées polaires $(r, \theta)$ d'origine $T$, où $\theta$ est l'angle entre $\overrightarrow{TM}$ et $\overrightarrow{TA}$. En utilisant $D \gg r$ (la distance Terre–astre est très grande devant le rayon terrestre), on développe $\vec{C}(M)$ au premier ordre en $r/D$.

Il est plus élégant de calculer d'abord le **potentiel de marée** $\Phi$ tel que $\vec{C} = -\vec{\nabla}\Phi$ :

$$\Phi(M) = -G\frac{m_A}{AM} + G\frac{m_A z}{D^2}$$

où $z = r\cos\theta$. En développant $AM^{-1}$ au second ordre en $r/D$ :

$$\boxed{\Phi(r,\theta) \simeq \frac{G\,m_A\,r^2}{2D^3}\left(1 - 3\cos^2\theta\right)}$$

(le terme constant et le terme en $r/D$ disparaissent après développement). On en déduit les composantes du champ de marée en coordonnées cylindriques :

$$\boxed{C_r = C_0\!\left(3\cos^2\theta - 1\right)} \qquad \boxed{C_\theta = -\frac{3}{2}\,C_0\sin(2\theta)}$$

avec le préfacteur d'intensité :

$$C_0 = \frac{G\,m_A\,r}{D^3}$$

Ce préfacteur dépend de l'astre au travers de $m_A/D^3$, ce qui explique la **prépondérance de la Lune** malgré sa faible masse : la Lune est $\sim 400$ fois plus proche du Soleil, ce qui compense largement sa masse 30 millions de fois plus faible.

| Astre  | Masse $m_A$ (kg)        | Distance $D$ (m)          | $C_0$ (m/s²)              |
|--------|-------------------------|---------------------------|---------------------------|
| Lune   | $7{,}35 \times 10^{22}$ | $3{,}84 \times 10^{8}$    | $5{,}52 \times 10^{-7}$   |
| Soleil | $1{,}99 \times 10^{30}$ | $1{,}495 \times 10^{11}$  | $2{,}53 \times 10^{-7}$   |

On calcule $C_{0,\text{Lune}} / C_{0,\text{Soleil}} \approx 2{,}18$ : **l'effet de la Lune est environ deux fois celui du Soleil**.

---

## III. Le double bourrelet océanique et la marée semi-diurne

### 3.1 Modèle hydrostatique

Dans le modèle simplifié dit **hydrostatique**, on suppose la Terre entièrement recouverte d'eau, et l'océan à l'équilibre à chaque instant sous l'action combinée de la pesanteur $\vec{g}$ et du champ de marée $\vec{C}$. La surface libre coïncide alors avec une **équipotentielle** du champ total.

En notant $h(\theta)$ la dénivellation de la surface par rapport à la sphère de référence, la condition d'équipotentialité donne :

$$h(\theta) = -\frac{\Phi(R_T, \theta)}{g} = \frac{C_0 R_T}{2g}\!\left(3\cos^2\theta - 1\right)$$

La hauteur est **maximale** pour $\theta = 0$ et $\theta = \pi$ (côté face à l'astre et côté opposé), et **minimale** pour $\theta = \pi/2$. La Terre prend la forme d'un **ellipsoïde allongé** dans la direction de l'astre, avec un **double bourrelet** de part et d'autre.

Le marnage hydrostatique vaut :

$$H = h_{\max} - h_{\min} = \frac{3C_0 R_T}{2g}$$

ce qui donne numériquement $H_\text{Lune} \approx 54$ cm et $H_\text{Soleil} \approx 25$ cm — valeurs très inférieures aux marnages réels (plusieurs mètres à Brest), car le modèle hydrostatique néglige la dynamique des océans.

### 3.2 Origine du rythme semi-diurne (~12 h 25 min)

La rotation de la Terre sur elle-même fait défiler le double bourrelet devant un point $M_0$ fixe à la surface : $M_0$ traverse alternativement les zones de haute mer ($\theta \approx 0$ et $\theta \approx \pi$) et de basse mer ($\theta \approx \pi/2$), soit **deux marées hautes et deux marées basses par cycle**.

La Lune n'est pas fixe dans $\mathcal{R}_G$ : elle orbite autour de la Terre avec une période sidérale $T_L \approx 27{,}3$ jours. Dans le référentiel terrestre, la période apparente de la Lune est donc :

$$T_{\text{semi-diurne}} = \frac{T_T \cdot T_L}{2(T_L - T_T)} \approx 12\text{ h }25\text{ min}$$

d'où le **retard quotidien d'environ 50 minutes** des marées observé dans les marégrammes.

---

## IV. Les autres rythmes des marées

### 4.1 Rythme bimensuel — vives-eaux et mortes-eaux (~14,7 jours)

La Lune et le Soleil créent chacun leur propre champ de marée. Lors de la **nouvelle lune** (NL) et de la **pleine lune** (PL), les deux ellipsoïdes de marée sont **alignés** : les effets se cumulent, produisant des **vives-eaux** (marnage maximal). Lors des **quartiers** (PQ, DQ), les grands axes des deux ellipsoïdes sont perpendiculaires : les variations de potentiel se compensent partiellement, produisant des **mortes-eaux** (marnage minimal).

La compensation n'est jamais totale car le potentiel lunaire est environ deux fois plus grand que le potentiel solaire. Ce rythme bimensuel correspond à la demi-période de la **lunaison synodique** ($T_\text{synodique} \approx 29{,}5$ jours), avec un déphasage d'environ **36 heures** à Brest, appelé *âge de la marée*.

### 4.2 Rythme saisonnier — marées d'équinoxe

Le potentiel de marée complet, en tenant compte de la déclinaison $\delta_0$ de l'astre (son angle par rapport au plan de l'équateur), fait apparaître un terme semi-diurne proportionnel à $\cos^2\delta_0$.

Lors des **équinoxes**, le Soleil se trouve dans le plan de l'équateur ($\delta_0 = 0$) : son influence sur les marées est **maximale**. Les vives-eaux qui coïncident avec un équinoxe présentent donc une amplitude particulièrement élevée : ce sont les célèbres **marées d'équinoxe**.

À l'inverse, lors des **solstices**, la déclinaison solaire est maximale ($\delta_0 \approx 23°$) : l'effet du Soleil est réduit, et le contraste vives-eaux/mortes-eaux diminue.

Ce comportement se vérifie clairement sur le **marégramme annuel de Brest (2001)**, où les marnages les plus importants se concentrent autour des équinoxes de printemps (20 mars) et d'automne (22 septembre).

### 4.3 Rythme diurne (~24 h 50 min)

Lorsque la déclinaison de la Lune est non nulle ($\delta_0 \neq 0$), les deux marées hautes successives d'une même journée n'ont **pas la même amplitude** : on parle d'*inégalité diurne*. Ce phénomène introduit un **rythme diurne** de période $\approx 24$ h $50$ min, superposé au rythme semi-diurne dominant sur les côtes atlantiques françaises.

---

## V. Bilan : la superposition des ondes de marée

La hauteur d'eau en un port donné $M$ peut s'écrire comme une **superposition d'ondes harmoniques** :

$$h(M, t) = \sum_i h_i(M)\cos\!\left(q_i t + \varphi_i(M)\right)$$

où chaque onde $i$ est caractérisée par une pulsation $q_i$, une amplitude $h_i(M)$ et une phase $\varphi_i(M)$ qui dépendent du port. La composante dominante sur les côtes françaises est l'**onde M₂** (lunaire semi-diurne), de période $\approx 12$ h $25$ min.

Les données SHOM du port de Brest utilisées dans ce projet permettent d'observer expérimentalement l'ensemble de ces rythmes — semi-diurne, bimensuel et saisonnier — et de les confronter aux prédictions de ce modèle théorique.

---

*Sources : B. Simon, La marée océanique côtière (2007) ; cours PC\* de physique — Les marées ; données SHOM/REFMAR.*
