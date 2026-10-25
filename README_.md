<p align="center">
  <img src="assets/figures/logo.png" width="160" alt="ENSEA">
</p>

<h1 align="center">Onduleurs et Qualité de l'énergie</h1>
<h3 align="center">Simulations d'onduleurs sous PSIM</h3>

<p align="center">
  Compte rendu de TP — ENSEA<br>
  <strong>Alejandro MUNOZ GUTIERREZ</strong> · <strong>Houssam BEKRI</strong> — Groupe TP1
</p>

---

## Résumé

Ce dépôt rassemble le compte rendu d'un TP de simulation (logiciel **PSIM**) sur les onduleurs :

- **Onduleur monophasé** : commande pleine onde, MLI à commandes complémentaires totales, MLI à commandes décalées, dimensionnement du filtre LC de sortie, et fonctionnement réversible redresseur/onduleur avec asservissement (boucles de courant et de tension).
- **Onduleur triphasé** : charge RL, influence de $\delta\alpha$, de $\alpha_0$ et de la fréquence de découpage, puis machine asynchrone (démarrage, temps d'accélération, inversion du sens de rotation).

## Sommaire

1. [Onduleur monophasé](#1-onduleur-monophasé)
   1. [Onduleur pleine onde](#a-onduleur-pleine-onde)
   2. [MLI à commandes complémentaires totales](#b-onduleur-mli-à-commandes-complémentaires-totales)
   3. [MLI à commandes décalées](#c-onduleur-mli-à-commandes-décalées)
   4. [Réversibilité redresseur / onduleur](#d-réversibilité-redresseur--onduleur)
2. [Onduleur triphasé](#2-onduleur-triphasé)
   1. [Simulations avec charge RL](#a-simulations-avec-charge-rl)
   2. [Simulation sur machine asynchrone](#b-simulation-sur-machine-asynchrone)

---

## 1. Onduleur monophasé

### a. Onduleur pleine onde

#### Schéma de simulation

<p align="center">
  <img src="assets/figures/fig01.png" width="600" alt="Figure 01"><br>
  <em>Figure 0 : Schéma de puissance d'un onduleur monophasé avec une charge RL sur PSIM.</em>
</p>


<p align="center">
  <img src="assets/figures/fig02.png" width="600" alt="Figure 02"><br>
  <em>Figure 1 : Partie contrôle/commande du schéma de puissance.</em>
</p>


<p align="center">
  <img src="assets/figures/fig02b.png" width="700" alt="Figure 02b"><br>
  <em>Figure 2 : Tension aux bornes de la charge (V<sub>charge</sub>,V<sub>R</sub>) et courants (I<sub>in</sub>, I<sub>out</sub>).</em>
</p>


Le courant débité par la source de tension DC ($V_{DC}$) devient négatif, donc la source $V_{DC}$ doit obligatoirement être **réversible**.

#### Simulations

<p align="center">
  <img src="assets/figures/fig03.png" width="700" alt="Figure 03"><br>
  <em>Figure 3 : Courants dans T1 et T2 (au sens large : IGBT + diode).</em>
</p>


- **Phase 1** : le transistor T1 est ON et conduit normalement.
- **Phase 2** : T1 est OFF, la diode D2 évacue le courant de l'inductance (continuité du courant dans l'inductance).
- **Phase 3** : le transistor T2 est ON et conduit normalement.
- **Phase 4** : T2 est OFF, le courant de l'inductance est évacué par la diode D1 (continuité du courant dans l'inductance).

<table>
<tr><td align="center"><img src="assets/figures/fig04.png" width="380"><br><em>Figure 4 : Phase 1.</em></td><td align="center"><img src="assets/figures/fig05.png" width="380"><br><em>Figure 5 : Phase 2.</em></td></tr>
<tr><td align="center"><img src="assets/figures/fig06.png" width="380"><br><em>Figure 6 : Phase 3.</em></td><td align="center"><img src="assets/figures/fig07.png" width="380"><br><em>Figure 7 : Phase 4.</em></td></tr>
</table>


<p align="center">
  <img src="assets/figures/fig08.png" width="700" alt="Figure 08"><br>
  <em>Figure 8 : Puissances instantanées côté V<sub>DC</sub>, entre les bras de l'onduleur et côté charge R.</em>
</p>


#### FFT

<p align="center">
  <img src="assets/figures/fig09.png" width="700" alt="Figure 09"><br>
  <em>Figure 9 : FFT des tensions de sortie (entre bras et aux bornes de R) et du courant de charge (jusqu'à 2 kHz).</em>
</p>


### b. Onduleur MLI à commandes complémentaires totales

<p align="center">
  <img src="assets/figures/fig10.png" width="600" alt="Figure 10"><br>
  <em>Figure 10 : Schéma de puissance d'un onduleur monophasé avec une charge RLC sur PSIM.</em>
</p>


<p align="center">
  <img src="assets/figures/fig11.png" width="600" alt="Figure 11"><br>
  <em>Figure 11 : Schéma de commande permettant de générer une commande complémentaire totale des interrupteurs.</em>
</p>


#### Évolution théorique de $\alpha(t)$

$$\langle V_{out} \rangle = (2\alpha - 1)\, V_{DC}$$

Avec $\alpha = 0.5 + \delta\alpha \sin(\omega t)$ :

$$\langle V_{out} \rangle = \left[ 2\left(0.5 + \delta\alpha \sin(\omega t)\right) - 1 \right] V_{DC} = 2\,\delta\alpha \sin(\omega t)\, V_{DC}$$

Pour une valeur efficace de 230 V, avec $V_{DC} = 400$ V :

$$2\,\delta\alpha\, V_{DC} = 230\sqrt{2} \quad\Rightarrow\quad \delta\alpha = \frac{230\sqrt{2}}{2 \times 400} = 0.4065$$

Pour recréer une tension sinusoïdale de valeur efficace 230 V aux bornes de la résistance de charge, il faut donc :

$$\alpha(t) = 0.5 + 0.4065 \sin(\omega t)$$

#### Dimensionnement de L et C

On détermine L et C pour que la fréquence de cassure du filtre de sortie soit de **500 Hz** et le coefficient d'amortissement **m = 0,8**.

<p align="center">
  <img src="assets/figures/fig12.png" width="350" alt="Figure 12"><br>
  <em>Figure 12 : Charge RLC.</em>
</p>


Impédance équivalente de l'ensemble C // R :

$$Z_{eq} = \frac{R \cdot \frac{1}{pC}}{R + \frac{1}{pC}} = \frac{R}{pRC + 1}$$

Pont diviseur de tension pour obtenir la fonction de transfert :

$$\frac{V_{out}}{U} = \frac{\frac{R}{pRC+1}}{\frac{R}{pRC+1} + pL} = \frac{R}{p^2 RLC + pL + R} = \frac{1}{p^2 LC + p\frac{L}{R} + 1}$$

Par identification avec un système du second ordre :

$$f_0 = \frac{1}{2\pi\sqrt{LC}} \qquad\text{et}\qquad m = \frac{L}{2R}\cdot\frac{1}{\sqrt{LC}}$$

<p align="center">
  <img src="assets/figures/fig13.jpeg" width="600" alt="Figure 13"><br>
  <em>Figure 13 : Résolution de L et C.</em>
</p>


$$L = 15.28\ \text{mH} \qquad\text{et}\qquad C = 6.63\ \mu\text{F}$$

#### Simulations

<p align="center">
  <img src="assets/figures/fig14.png" width="700" alt="Figure 14"><br>
  <em>Figure 14 : Tension entre bras de pont et aux bornes de R.</em>
</p>


<p align="center">
  <img src="assets/figures/fig15.png" width="700" alt="Figure 15"><br>
  <em>Figure 15 : Courant dans l'inductance L.</em>
</p>


<p align="center">
  <img src="assets/figures/fig16.png" width="700" alt="Figure 16"><br>
  <em>Figure 16 : Courant débité par la source V<sub>DC</sub>.</em>
</p>


#### Tension entre bras filtrée

<p align="center">
  <img src="assets/figures/fig17.png" width="700" alt="Figure 17"><br>
  <em>Figure 17 : Comparaison entre la tension entre bras filtrée et la tension à la charge.</em>
</p>


#### FFT

<p align="center">
  <img src="assets/figures/fig18.png" width="700" alt="Figure 18"><br>
  <em>Figure 18 : FFT de la tension aux bornes de la charge (V<sub> R</sub>) et de la tension entre les bras de pont (V<sub> charge</sub>).</em>
</p>


Le filtre LC fonctionne comme prévu : il supprime les harmoniques au-delà de sa fréquence de coupure de 500 Hz (figure 18, graphique du haut).

### c. Onduleur MLI à commandes décalées

<p align="center">
  <img src="assets/figures/fig19.png" width="600" alt="Figure 19"><br>
  <em>Figure 19 : Schéma de commande permettant de générer une commande décalée des interrupteurs.</em>
</p>


#### Simulations

<p align="center">
  <img src="assets/figures/fig20.png" width="700" alt="Figure 20"><br>
  <em>Figure 20 : PWM pour contrôler les interrupteurs T1 et T3 (commandes décalées).</em>
</p>


<p align="center">
  <img src="assets/figures/fig21.png" width="700" alt="Figure 21"><br>
  <em>Figure 21 : Tension entre bras de pont V<sub>charge </sub> et aux bornes de la charge V<sub>R </sub> (commandes décalées).</em>
</p>


<p align="center">
  <img src="assets/figures/fig22.png" width="700" alt="Figure 22"><br>
  <em>Figure 22 : Courant dans l'inductance (commandes décalées).</em>
</p>


<p align="center">
  <img src="assets/figures/fig23.png" width="700" alt="Figure 23"><br>
  <em>Figure 23 : Courant débité par la source V<sub>DC </sub> (commandes décalées).</em>
</p>


#### Tension entre bras filtrée

<p align="center">
  <img src="assets/figures/fig24.png" width="700" alt="Figure 24"><br>
  <em>Figure 24 : Tensions entre bras filtrée et de sortie (commandes décalées).</em>
</p>


#### FFT

<p align="center">
  <img src="assets/figures/fig25.png" width="700" alt="Figure 25"><br>
  <em>Figure 25 : FFT de la tension aux bornes de la charge (V<sub>R </sub>) et de la tension entre les bras de pont (V<sub>charge </sub>) (commandes décalées).</em>
</p>


En comparant avec les commandes complémentaires totales, on constate qu'avec des commandes décalées les harmoniques commencent à **10 kHz** au lieu de **5 kHz**.

### d. Réversibilité redresseur / onduleur

<p align="center">
  <img src="assets/figures/fig26.png" width="700" alt="Figure 26"><br>
  <em>Figure 26 : Étude du fonctionnement en redresseur MLI — boucle ouverte.</em>
</p>


On calcule le rapport cyclique pour que :

1. la puissance transférée de $V_1$ vers la résistance de charge soit de **2 kW** ;
2. la tension $V_{DC}$ aux bornes de la résistance soit de **500 V** ;
3. le courant $I_1$ (débité par $V_1$) soit **en phase** avec la tension $V_1$.

<p align="center">
  <img src="assets/figures/fig27.png" width="550" alt="Figure 27"><br>
  <em>Figure 27 : Schéma d'un onduleur réversible.</em>
</p>


$$U = V_o + jL\omega I \qquad\Rightarrow\qquad V_o = U - jL\omega I$$

<p align="center">
  <img src="assets/figures/fig28.png" width="550" alt="Figure 28"><br>
  <em>Figure 28 : Diagramme de Fresnel du redresseur.</em>
</p>


Le courant et la tension doivent être en phase, donc $\cos\varphi = 1$ et :

$$I = \frac{P}{U} = \frac{2000}{110} = 18.18\ \text{A}$$

Déphasage entre $U$ et $V_o$ (source à 60 Hz) :

$$\theta = \arg(V_o) = \arctan\!\left(\frac{-L\omega I}{U}\right) = -31.92^\circ$$

Pour obtenir $V_{DC} = 500$ V :

$$V_o = \frac{2\, V_{DC}\, \delta\alpha}{\sqrt{2}} \quad\Rightarrow\quad \delta\alpha = \frac{V_o \sqrt{2}}{2\, V_{DC}} = \frac{\sqrt{U^2 + (L\omega I)^2}\,\sqrt{2}}{2\, V_{DC}}$$

$$\delta\alpha = \frac{\sqrt{110^2 + (0.01 \times 2\pi \times 60 \times 18.18)^2}\;\sqrt{2}}{2 \times 500} = 0.183$$

#### Fonctionnement en boucle ouverte

<p align="center">
  <img src="assets/figures/fig29.png" width="700" alt="Figure 29"><br>
  <em>Figure 29 : Tension V<sub>1 </sub> et courant I<sub>L1 </sub>) en boucle ouverte.</em>
</p>


<p align="center">
  <img src="assets/figures/fig30.png" width="700" alt="Figure 30"><br>
  <em>Figure 30 : Tension V<sub>o</sub> (entre bras) et sa version filtrée V<sub>o_filtre </sub>.</em>
</p>


<p align="center">
  <img src="assets/figures/fig31.png" width="700" alt="Figure 31"><br>
  <em>Figure 31 : Courant I<sub>DC </sub> et sa version filtrée I<sub>DC_filtre</sub>.</em>
</p>


<p align="center">
  <img src="assets/figures/fig32.png" width="700" alt="Figure 32"><br>
  <em>Figure 32 : Puissance moyenne côté réseau, courant I<sub>L 1</sub> et tension V<sub>1 </sub>.</em>
</p>


<p align="center">
  <img src="assets/figures/fig33.png" width="700" alt="Figure 33"><br>
  <em>Figure 33 : Tension du bus continu V<sub>DC_bus </sub>, courant I<sub>DC </sub> et puissance moyenne.</em>
</p>


<p align="center">
  <img src="assets/figures/fig34.png" width="700" alt="Figure 34"><br>
  <em>Figure 34 : Comparaison des puissances moyennes côté alternatif et côté continu.</em>
</p>


#### Asservissement du courant

<p align="center">
  <img src="assets/figures/fig35.png" width="700" alt="Figure 35"><br>
  <em>Figure 35 : Schéma de simulation avec boucle de courant (régulateur PI et génération du rapport cyclique).</em>
</p>


<p align="center">
  <img src="assets/figures/fig36.png" width="700" alt="Figure 36"><br>
  <em>Figure 36 : Courant I<sub>L1 </sub> et tension V<sub>1 </sub> en régime établi.</em>
</p>


<p align="center">
  <img src="assets/figures/fig37.png" width="700" alt="Figure 37"><br>
  <em>Figure 37 : Courant I<sub>L1 </sub> et tension V<sub>1 </sub> (échelle de temps plus large).</em>
</p>


<p align="center">
  <img src="assets/figures/fig38.png" width="700" alt="Figure 38"><br>
  <em>Figure 38 : Puissance moyenne.</em>
</p>


<p align="center">
  <img src="assets/figures/fig39.png" width="700" alt="Figure 39"><br>
  <em>Figure 39 : Tension du bus continu V<sub>DC_bus </sub> (ondulation autour de 500 V).</em>
</p>


#### Boucle de tension et de courant

<p align="center">
  <img src="assets/figures/fig40.png" width="700" alt="Figure 40"><br>
  <em>Figure 40 : Schéma de simulation avec boucle de tension (externe) et boucle de courant (interne).</em>
</p>


**Charge 125 Ω**

<p align="center">
  <img src="assets/figures/fig41.png" width="700" alt="Figure 41"><br>
  <em>Figure 41 : Tension du bus continu, courant I<sub>L1 </sub> et tension V<sub>1 </sub> (charge 125 Ω).</em>
</p>


<p align="center">
  <img src="assets/figures/fig42.png" width="700" alt="Figure 42"><br>
  <em>Figure 42 : Tension du bus continu, courant, tension V<sub>1 </sub> et puissance moyenne (charge 125 Ω).</em>
</p>


**Charge 250 Ω**

<p align="center">
  <img src="assets/figures/fig43.png" width="700" alt="Figure 43"><br>
  <em>Figure 43 : Tension du bus continu, courant V<sub>L1 </sub> et tension V<sub>1 </sub> (charge 250 Ω).</em>
</p>


<p align="center">
  <img src="assets/figures/fig44.png" width="700" alt="Figure 44"><br>
  <em>Figure 44 : Tension du bus continu, courant, tension V<sub>1 </sub> et puissance moyenne (charge 250 Ω).</em>
</p>


---

## 2. Onduleur triphasé

### a. Simulations avec charge RL

<p align="center">
  <img src="assets/figures/fig45.png" width="700" alt="Figure 45"><br>
  <em>Figure 45 : Schéma de puissance d'un onduleur triphasé avec une charge RL.</em>
</p>


<p align="center">
  <img src="assets/figures/fig46.png" width="550" alt="Figure 46"><br>
  <em>Figure 46 : Logique de commande des interrupteurs de l'onduleur triphasé.</em>
</p>


Pour commander les interrupteurs d'un onduleur triphasé, il faut 3 signaux modulants déphasés de 120° les uns par rapport aux autres (un par bras). Chaque rapport cyclique est de la forme :

$$\alpha(t) = \alpha_0 + \delta\alpha \sin(\Omega t + \varphi)$$

- $\alpha_0$ : offset du rapport cyclique ($\alpha_0 = 0.5$) ;
- $\delta\alpha$ : amplitude maximale du rapport cyclique ($\delta\alpha = 0.4$) ;
- $\Omega$ : pulsation du signal ;
- $\varphi$ : déphasage du signal (0°, −120°, −240°).

Chaque signal est comparé à un signal triangulaire de fréquence de découpage **5 kHz** pour générer la commande des transistors.

<p align="center">
  <img src="assets/figures/fig47.png" width="700" alt="Figure 47"><br>
  <em>Figure 47 : Commande de T1.</em>
</p>


<p align="center">
  <img src="assets/figures/fig48.png" width="700" alt="Figure 48"><br>
  <em>Figure 48 : Les trois courants de charge.</em>
</p>


<p align="center">
  <img src="assets/figures/fig49.png" width="700" alt="Figure 49"><br>
  <em>Figure 49 : Tension entre le point milieu du premier bras (T1/T4) et la masse.</em>
</p>


<p align="center">
  <img src="assets/figures/fig50.png" width="700" alt="Figure 50"><br>
  <em>Figure 50 : Tension composée $U_{12}$.</em>
</p>


<p align="center">
  <img src="assets/figures/fig51.png" width="700" alt="Figure 51"><br>
  <em>Figure 51 : Courant prélevé sur l'alimentation $I_{in}$.</em>
</p>


On obtient trois courants sinusoïdaux grâce à la fréquence de découpage élevée. Ils sont équilibrés, ce qui est dû à la charge équilibrée, et le courant maximal prélevé concorde avec l'amplitude des courants de phase.

La tension entre le point milieu du bras T1/T4 et la masse varie bien entre 0 et 600 V, exactement synchronisée avec le signal de commande de T1. La tension composée $U_{12}$ varie bien entre $0$ et $\pm V_{DC}$.

<p align="center">
  <img src="assets/figures/fig52.png" width="700" alt="Figure 52"><br>
  <em>Figure 52 : Mesure de la tension composée $U_{12}$ avec un filtre passe-bas du 1er puis du 2ème ordre (fréquence de coupure 500 Hz).</em>
</p>


<p align="center">
  <img src="assets/figures/fig53.png" width="600" alt="Figure 53"><br>
  <em>Figure 53 : Valeurs efficaces de la tension composée $U_{12}$ (en bleu) avec les 2 filtres.</em>
</p>


Ces mesures sont cohérentes avec la formule théorique de la valeur efficace d'une tension composée d'un onduleur triphasé :

$$U_{eff} = \frac{\sqrt{3}}{\sqrt{2}}\, \delta\alpha\, V_{DC}$$

<p align="center">
  <img src="assets/figures/fig54.png" width="550" alt="Figure 54"><br>
  <em>Figure 54 : $U_{eff\_filtrée}$ en fonction de &delta;&alpha;.</em>
</p>


La relation entre $U_{eff\_filtrée}$ et $\delta\alpha$ est proportionnelle et linéaire, ce qui confirme la formule.

#### Cas $\delta\alpha = 2$

<p align="center">
  <img src="assets/figures/fig55.png" width="700" alt="Figure 55"><br>
  <em>Figure 55 : Commande de T1 et trois courants de charge ($\delta\alpha = 2$).</em>
</p>


<p align="center">
  <img src="assets/figures/fig56.png" width="700" alt="Figure 56"><br>
  <em>Figure 56 : Tension entre le point milieu du bras 1 et la masse, tension composée $U_{12}$ et courant sur l'alimentation $I_{in}$ ($\delta\alpha = 2$).</em>
</p>


Avec $\delta\alpha = 2$, on constate une **saturation** du courant et des tensions par rapport aux mesures avec $\delta\alpha = 0.4$ : $\delta\alpha$ est trop élevé.

#### Influence de la fréquence de découpage

<p align="center">
  <img src="assets/figures/fig57.png" width="700" alt="Figure 57"><br>
  <em>Figure 57 : Courant de charge de la phase 1 et tension $U_{12}$ après filtrage par un passe-bas du 1er ordre (fréquence de découpage 5 kHz).</em>
</p>


<p align="center">
  <img src="assets/figures/fig58.png" width="700" alt="Figure 58"><br>
  <em>Figure 58 : FFT du courant de charge et de la tension $U_{12}$ après filtrage (fréquence de découpage 5 kHz).</em>
</p>


<p align="center">
  <img src="assets/figures/fig59.png" width="700" alt="Figure 59"><br>
  <em>Figure 59 : Courant de charge de la phase 1 et tension $U_{12}$ après filtrage par un passe-bas du 1er ordre (fréquence de découpage 500 Hz).</em>
</p>


<p align="center">
  <img src="assets/figures/fig60.png" width="700" alt="Figure 60"><br>
  <em>Figure 60 : FFT du courant de charge et de la tension $U_{12}$ après filtrage (fréquence de découpage 500 Hz).</em>
</p>


Changer la fréquence de découpage permet de contrôler le nombre d'harmoniques : plus on l'augmente, moins il y en a.

#### Influence de $\alpha_0$

<p align="center">
  <img src="assets/figures/fig61.png" width="600" alt="Figure 61"><br>
  <em>Figure 61 : Courants de charge avec $\alpha_0 = 0.4,\ 0.5,\ 0.6$.</em>
</p>


<p align="center">
  <img src="assets/figures/fig62.png" width="600" alt="Figure 62"><br>
  <em>Figure 62 : Tension composée $U_{12}$ avec $\alpha_0 = 0.4,\ 0.5,\ 0.6$.</em>
</p>


<p align="center">
  <img src="assets/figures/fig63.png" width="600" alt="Figure 63"><br>
  <em>Figure 63 : Courant prélevé sur l'alimentation $I_{in}$ avec $\alpha_0 = 0.4,\ 0.5,\ 0.6$.</em>
</p>


Même en changeant $\alpha_0$, on obtient à peu près les mêmes courbes pour les courants de phase, la tension composée $U_{12}$ et le courant prélevé sur l'alimentation $I_{in}$.

### b. Simulation sur machine asynchrone

La charge de l'onduleur est maintenant une machine asynchrone à cage (*Squirrel Cage Induction Machine*) associée à une charge mécanique (*Mechanical Load (general)*).

<p align="center">
  <img src="assets/figures/fig64.png" width="700" alt="Figure 64"><br>
  <em>Figure 64 : Schéma de simulation de l'onduleur triphasé alimentant la machine asynchrone.</em>
</p>


#### Cas $\delta\alpha = 0.5$

<p align="center">
  <img src="assets/figures/fig65.png" width="700" alt="Figure 65"><br>
  <em>Figure 65 : Courants des phases du moteur ($\delta\alpha = 0.5$).</em>
</p>


Pour le rapport du courant de démarrage sur le courant de régime établi ($I_d / I_n$), on prend comme valeur de $I_d$ la valeur efficace juste après le tout début du démarrage :

$$\frac{I_d}{I_n} = \frac{14}{3.84} = 3.65$$

<p align="center">
  <img src="assets/figures/fig66.png" width="700" alt="Figure 66"><br>
  <em>Figure 66 : Vitesse de rotation du moteur ($\delta\alpha = 0.5$).</em>
</p>


<p align="center">
  <img src="assets/figures/fig67.png" width="700" alt="Figure 67"><br>
  <em>Figure 67 : Couple instantané et couple moyenné sur l'arbre ($\delta\alpha = 0.5$).</em>
</p>


<p align="center">
  <img src="assets/figures/fig68.png" width="700" alt="Figure 68"><br>
  <em>Figure 68 : Puissance mécanique moyennée ($\delta\alpha = 0.5$).</em>
</p>


<p align="center">
  <img src="assets/figures/fig69.png" width="700" alt="Figure 69"><br>
  <em>Figure 69 : Puissance électrique instantanée absorbée par l'onduleur ($\delta\alpha = 0.5$).</em>
</p>


<p align="center">
  <img src="assets/figures/fig70.png" width="700" alt="Figure 70"><br>
  <em>Figure 70 : Vitesse et accélération du moteur ($\delta\alpha = 0.5$).</em>
</p>


Le temps d'accélération est d'environ **0,77 s**.

#### Cas $\delta\alpha = 0.3$

Lors de la première simulation, le régime permanent n'était pas atteint ; la simulation a donc été allongée à 6 secondes.

<p align="center">
  <img src="assets/figures/fig71.png" width="700" alt="Figure 71"><br>
  <em>Figure 71 : Courants des phases du moteur ($\delta\alpha = 0.3$).</em>
</p>


$$\frac{I_d}{I_n} = \frac{8.35}{3.81} = 2.19$$

<p align="center">
  <img src="assets/figures/fig72.png" width="700" alt="Figure 72"><br>
  <em>Figure 72 : Vitesse de rotation du moteur ($\delta\alpha = 0.3$).</em>
</p>


<p align="center">
  <img src="assets/figures/fig73.png" width="700" alt="Figure 73"><br>
  <em>Figure 73 : Couple instantané et couple moyenné sur l'arbre ($\delta\alpha = 0.3$).</em>
</p>


<p align="center">
  <img src="assets/figures/fig74.png" width="700" alt="Figure 74"><br>
  <em>Figure 74 : Puissance mécanique moyennée ($\delta\alpha = 0.3$).</em>
</p>


<p align="center">
  <img src="assets/figures/fig75.png" width="700" alt="Figure 75"><br>
  <em>Figure 75 : Puissance électrique instantanée absorbée par l'onduleur ($\delta\alpha = 0.3$).</em>
</p>


<p align="center">
  <img src="assets/figures/fig76.png" width="700" alt="Figure 76"><br>
  <em>Figure 76 : Vitesse et accélération du moteur ($\delta\alpha = 0.3$).</em>
</p>


Le temps d'accélération est d'environ **3,87 s**.

#### Comparaison

| | $\delta\alpha = 0.5$ | $\delta\alpha = 0.3$ |
|---|:---:|:---:|
| Rapport $I_d / I_n$ | 3.65 | 2.19 |
| Temps d'accélération (s) | 0.77 | 3.87 |

$\delta\alpha$ influence à la fois le temps d'accélération et le courant de démarrage demandé par le moteur. On observe aussi un dépassement plus important de la puissance mécanique moyennée pour $\delta\alpha = 0.5$ que pour $\delta\alpha = 0.3$.

#### Inversion du sens de rotation

Pour changer le sens de rotation, on inverse l'ordre de deux des trois phases (séquence directe ABC → séquence inverse ACB), ce qui inverse instantanément le sens de rotation du champ magnétique statorique.

On laisse la phase A telle quelle et on ajoute deux multiplexeurs pour les phases B et C : à t = 1,1 s, un échelon passe de 0 à 1 V, ce qui fait changer d'entrée les multiplexeurs et permute donc les commandes des phases B et C.

<p align="center">
  <img src="assets/figures/fig77.png" width="550" alt="Figure 77"><br>
  <em>Figure 77 : Schéma de commande pour l'inversion du sens de rotation du moteur.</em>
</p>


<p align="center">
  <img src="assets/figures/fig78.png" width="700" alt="Figure 78"><br>
  <em>Figure 78 : Couple moyenné sur l'arbre, vitesse de rotation du moteur et puissance mécanique moyennée.</em>
</p>


<p align="center">
  <img src="assets/figures/fig79.png" width="700" alt="Figure 79"><br>
  <em>Figure 79 : Courants de phases du moteur.</em>
</p>


<p align="center">
  <img src="assets/figures/fig80.png" width="700" alt="Figure 80"><br>
  <em>Figure 80 : Courants de phases du moteur lors de l'inversion des phases B et C.</em>
</p>


Observations :

- La vitesse de rotation diminue jusqu'à devenir négative puis atteint sa valeur maximale négative : le sens de rotation s'est bien inversé.
- Le couple moyenné sur l'arbre a à peu près la même allure à vitesse négative qu'à vitesse positive.
- La puissance mécanique devient négative pendant le freinage, puis a exactement la même allure pendant l'accélération en sens inverse que pendant l'accélération en sens direct.
- Le courant met à peu près le double de temps pour atteindre le régime permanent, ce qui est normal puisque le moteur passe de −vitesse max à +vitesse max. À 1,1 s, la phase A ne change pas brusquement tandis que les phases B et C changent soudainement, à cause de la permutation des phases B et C.
- La valeur efficace du courant après permutation ressemble à celle du démarrage, à cause du couple nécessaire pour freiner le moteur puis l'accélérer jusqu'à la vitesse maximale négative ; elle redevient égale au courant RMS du régime permanent.

---

## Outils

- Simulation : **PSIM**
- Rédaction d'origine : Word (`.docx`) / PDF

<sub>Compte rendu réalisé à l'ENSEA — Onduleurs et Qualité de l'énergie.</sub>
