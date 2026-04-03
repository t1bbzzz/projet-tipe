# Partie théorique — Origines physiques des marées

## I. Choix du référentiel d'étude

Pour modéliser correctement le phénomène des marées, le choix du référentiel est crucial.

Le **référentiel de Copernic** $\mathcal{R}_C$, centré sur le Soleil avec des axes pointant vers des étoiles lointaines, est considéré comme **galiléen** à l'échelle du système solaire.

Le **référentiel géocentrique** $\mathcal{R}_G$, centré sur la Terre $T$ avec des axes parallèles à ceux de Copernic, est en **translation** par rapport à $\mathcal{R}_C$. Cependant, puisque le centre de la Terre décrit une ellipse autour du Soleil, son centre $T$ possède une accélération non nulle dans $\mathcal{R}_C$ :

$$\vec{a}_T \neq \vec{0}$$

Le référentiel géocentrique est donc **non galiléen**. C'est précisément cette non-galiléité qui est à l'origine du phénomène des marées.

---

## II. Application du Principe Fondamental de la Dynamique

On étudie une particule de fluide marin de masse $m$ en un point $M$ de la surface terrestre. On applique le **PFD dans $\mathcal{R}_G$**, qui est non galiléen. Le bilan des forces s'écrit :

$$m\,\vec{a}(M)_{\mathcal{R}_G} = m\,\vec{g}_T(M) + m\,\vec{g}_L(M) + m\,\vec{g}_S(M) + \vec{F}_{ie}$$

où :

- $m\,\vec{g}_T(M)$ : attraction gravitationnelle de la Terre sur la particule en $M$
- $m\,\vec{g}_L(M)$ : attraction gravitationnelle de la Lune sur la particule en $M$
- $m\,\vec{g}_S(M)$ : attraction gravitationnelle du Soleil sur la particule en $M$
- $\vec{F}_{ie} = -m\,\vec{a}_e$ : force d'inertie d'entraînement due à la non-galiléité de $\mathcal{R}_G$

La force de Coriolis est négligée ici (on s'intéresse à l'équilibre statique du bourrelet, pas aux courants de marée).

L'accélération d'entraînement $\vec{a}_e$ est l'accélération du centre $T$ de la Terre dans $\mathcal{R}_C$. Pour la calculer, on applique le **Théorème du Centre d'Inertie** à la Terre entière dans $\mathcal{R}_C$ :

$$M_T\,\vec{a}_T = M_T\,\vec{g}_L(T) + M_T\,\vec{g}_S(T)$$

soit :

$$\vec{a}_e = \vec{a}_T = \vec{g}_L(T) + \vec{g}_S(T)$$

Le centre de la Terre est accéléré par l'attraction combinée de la Lune et du Soleil *en son centre géométrique*.

---

## III. Émergence du terme différentiel de marée

En substituant $\vec{a}_e$ dans le PFD de la particule d'eau, et en regroupant les termes :

$$m\,\vec{a}(M)_{\mathcal{R}_G} = m\,\vec{g}_T(M) + m\underbrace{\left[\vec{g}_L(M) - \vec{g}_L(T)\right]}_{\Delta\vec{g}_L(M)} + m\underbrace{\left[\vec{g}_S(M) - \vec{g}_S(T)\right]}_{\Delta\vec{g}_S(M)}$$

On définit ainsi le **terme différentiel de marée** :

$$\boxed{\Delta\vec{g}_L(M) = \vec{g}_L(M) - \vec{g}_L(T) \qquad \Delta\vec{g}_S(M) = \vec{g}_S(M) - \vec{g}_S(T)}$$

**Interprétation physique :** la marée est causée par la *différence* entre l'attraction gravitationnelle de la Lune (ou du Soleil) sur la particule d'eau en $M$, et celle exercée sur le centre de la Terre $T$. Si le champ lunaire était uniforme sur toute la Terre, cette différence serait nulle et il n'y aurait aucune marée.

> **Remarque importante :** c'est également pourquoi on autorise souvent à traiter le référentiel géocentrique comme approximativement galiléen en mécanique spatiale. La force d'inertie d'entraînement compense globalement l'attraction du Soleil et de la Lune sur la Terre. Les marées ne sont que le petit résidu différentiel de cette compensation.

---

## IV. Ordre de grandeur — décroissance en $1/d^3$

On estime l'intensité du terme différentiel lunaire $\Delta\vec{g}_L$ par un **développement limité** au premier ordre en $R_T/d$, avec $R_T$ le rayon terrestre et $d$ la distance Terre-Lune ($R_T \ll d$).

En considérant le point $M$ aligné avec $T$ et la Lune (cas le plus favorable), la différence d'attraction vaut :

$$\Delta g_L = \frac{GM_L}{(d - R_T)^2} - \frac{GM_L}{d^2} = \frac{GM_L}{d^2}\left[\left(1 - \frac{R_T}{d}\right)^{-2} - 1\right]$$

En développant au premier ordre en $R_T/d \ll 1$ :

$$\left(1 - \frac{R_T}{d}\right)^{-2} \simeq 1 + \frac{2R_T}{d}$$

On obtient la formule approchée :

$$\boxed{\Delta g_L \approx \frac{2\,G\,M_L\,R_T}{d^3}}$$

**Deux enseignements majeurs :**

1. Le terme de marée décroît en **$1/d^3$**, et non en $1/d^2$ comme l'attraction gravitationnelle ordinaire. La distance joue donc un rôle encore plus déterminant pour les marées que pour la gravitation simple.

2. Numériquement : $\Delta g_L \sim 10^{-6}\ \mathrm{m/s^2} \approx 10^{-7}\,g$. C'est une accélération extrêmement faible, mais suffisante pour déformer les océans à l'échelle planétaire sur des temps longs.

---

## V. Formation du double bourrelet — deux marées par jour

La structure du terme différentiel $\Delta\vec{g}_L(M)$ explique directement la géographie des marées haute et basse :

- **Côté face à la Lune** : $M$ est plus proche de la Lune que $T$, donc $|\vec{g}_L(M)| > |\vec{g}_L(T)|$. Le vecteur $\Delta\vec{g}_L$ est dirigé *vers la Lune* : l'eau est tirée vers l'extérieur → **marée haute**.

- **Côté opposé à la Lune** : $M$ est plus éloigné de la Lune que $T$, donc $|\vec{g}_L(M)| < |\vec{g}_L(T)|$. La force d'inertie d'entraînement l'emporte et $\Delta\vec{g}_L$ est dirigé *à l'opposé de la Lune* : l'eau est également repoussée vers l'extérieur → **marée haute**.

- **Sur les côtés (à 90°)** : l'effet différentiel ramène l'eau vers le centre de la Terre → **marée basse**.

Il se forme ainsi un **double bourrelet océanique** ellipsoïdal, symétrique de part et d'autre de la Terre dans la direction Terre-Lune. La Terre tournant sur elle-même à l'intérieur de ce bourrelet (quasi-fixe dans $\mathcal{R}_G$), un point fixe à la surface traverse **deux bourrelets hauts et deux zones basses par jour** : c'est le rythme **semi-diurne** d'environ $12\ \mathrm{h}\ 25\ \mathrm{min}$.

**Le décalage de 50 minutes par jour :** la Lune n'est pas fixe dans $\mathcal{R}_G$, elle tourne autour de la Terre en $\approx 27{,}3$ jours. Pendant qu'un point de la Terre boucle une rotation (24 h solaires), la Lune a légèrement avancé sur son orbite. Il faut donc environ **50 minutes supplémentaires** pour retrouver la même configuration Terre-Lune. La période semi-diurne réelle est ainsi $T \approx 12\ \mathrm{h}\ 25\ \mathrm{min}$, directement observable sur les données SHOM de Brest.

---

## VI. Lune vs Soleil — vives-eaux et mortes-eaux

D'après la formule approchée $\Delta g \approx \frac{2GM\,R_T}{d^3}$, le rapport des termes différentiels lunaire et solaire vaut :

$$\frac{\Delta g_L}{\Delta g_S} = \frac{M_L}{M_S} \times \left(\frac{d_S}{d_L}\right)^3 \approx \frac{7{,}35 \times 10^{22}}{1{,}99 \times 10^{30}} \times \left(\frac{1{,}50 \times 10^{11}}{3{,}84 \times 10^{8}}\right)^3 \approx 2{,}2$$

Bien que le Soleil soit environ $27$ millions de fois plus massif que la Lune, la Lune est $\approx 390$ fois plus proche, et c'est la dépendance en **$d^3$** qui s'avère déterminante. Le terme de marée lunaire est donc environ **2,2 fois plus intense** que le terme solaire : **la Lune est le moteur principal des marées.**

Ces deux contributions se combinent selon la géométrie du système Soleil-Terre-Lune :

- **Nouvelle lune / Pleine lune** : les trois corps sont alignés. Les bourrelets lunaire et solaire se superposent → effets **additifs** → **vives-eaux** (marnage maximal).

- **Premier / Dernier quartier** : Lune et Soleil forment un angle droit avec la Terre. Le bourrelet solaire vient en opposition partielle avec le bourrelet lunaire → effets **partiellement compensés** → **mortes-eaux** (marnage minimal). La compensation est incomplète car $\Delta g_L \approx 2{,}2\,\Delta g_S$.
