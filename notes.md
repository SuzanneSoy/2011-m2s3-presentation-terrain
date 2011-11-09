Perlin noise
============
Quatre shémas
- Interpolation aucune.
- Interpolation linéaire.
- Interpolation cubique.
- Interpolation cosinusoïdale.
Autres interpolations possibles vues dans le dernier cours (présentation par l'autre groupe).

superposition d'octaves (deux autres).
Sommes des octaves sur exemple en interpolation linéaire.
Un exemple final avec une interpolation cubique.
--> retour au schéma trois courbes superposées.
Amplitude : Hauteur max pour une octave.
Octaves : Nombre d'octaves qui seront additionnées.
Fréquence : La fréquence déchantillonage pour chaque octave (un point tout les x).
Persistance : Variation de l'amplitude d'une octave à l'autre.

Hash de coordonnées : pour un point d'une octave on fait hash(coordonnées) % amplitude de l'octave. Avec des octaves de fréquence double à chaque niveau il suffit de faire un modulo pour conaître les coordonnées de l'octave parente.


Perlin noise (variations)
=========================
Généralisation à n dimensions.
exemple 1D et 2D.
(peut-être exemple 3D)
Si quatre dimension alors animations.
exemple à citer en plus : minefield : seuil sur un bruit 3D pour "colorer" des voxels.


Ridged perlin noise : Fait des crêtes de montagnes ou vallées encaissées.
Un exemple de crêtes et un exemple de vallées.
Midpoint displacement : fractale des côtes.
Simplex Noise : Explication sur deux dimension avec un carré et un triangle pour comparaison simplex noise étant le triangle.
Simplex Noise : Complexité meilleure : cube = 8 points, tétraèdre = 4 points.
Simplex Noise : On peut connaître sa dérivée ! Donc normales faciles

pour économiser de la mémoire il est possible de générer des textures répétables.
Après génération moyenne des bords (monde torique).
problèmes : flou principalement sur les bords et artefacts.
Une solution : Généralisation à n dimensions : hypercercle nD dans un espace 2nD.

Craters et hills algorithm
==========================
Craters : Création d'un cratère : z(x,y) = z(x,y) en cours - une hauteur déterminée par la distance au centre (en général décroissant).
On peut générer un terrain juste avec des cratères (carte de 512x512 environ un milier de cratères).
Ou ajout de cratères sur un terrain existant pour ajouter de l'intérêt.

Hills : inverse de craters ajout de cercles.

Stockage : Possibilité de faire un QuadTree de carrés avec un cercle positionné aléatoirement dans chaque niveau dans ce cas là complexité comparable avec celle de perlin noise.
Avec arbre du LOD les cercles des cratères ne doivent pas dépasser du carré.

Erosion
=======
Idée générale déplacement de sédiments. 
Taux d'érosion : quantité de sédiments déplacés.
Pente : dérivée du simplex noise ou différence sur perlin.
Dureté : un autre perlin noise.
Végétation (climat) : fonction de l'altitude distance à l'eau et bruit aléatoire.
Inconvénient : pas de temps réel.
- Approximation : modification de la distribution des hauteurs.


Autres méthodes
===============
Chaînage d'algorithme de bruit : permet d'ajouter des couleurs, des climats etc à une carte d'élévation.
altération du comportement d'un aglo : Initialisation d'un algo à partir du résultat d'un autre algo. (exemple utiliser le résultat de perlin noise pour déterminer la persistance pour un second perlin noise).
Carte polygonale :
- découpage du plan en polygones
- positionnement de la mer et des lacs
- hauteur fonction de la distance à la mer.
- tracé de rivières en descendant le long des segments des polygones.
- climats et biotopes en fonction de l'élévation et de la distance à l'humidité.
- bruitage supplémentaire.

Intégration de formes dans le terrain : modification du comportement de l'algo et retour à l'algo normal en fonction de la distance. Permet de rajoute du réalisme fait par un graphiste (exemple grande muraille de chine, continents réels).


Rivières
========
- Path finding : Une recherche de chemin avec un coût en fonction de la pente.
- Affinage : Série de ponts de passage placés sur la grille de LOD avec une précision que l'on augmente quand on affine le LOD. On rajoute des points de passage et ajuste ceux existants.
- Tracé arbitraire : Choix du placement et du tracé de la rivière sans prendre en compte le terrain.
- Intégration dans le terrain : De la même manière que la diapo précédente, la rivère constitue une forme spéciale à intégrer. Problème risque de faire passer une rivière au milieu d'un massif (vallée très très très très artificielle).

Démonstration
=============
World Machine.


Rendu
=====

Isosurfaces
-----------



Ray casting
-----------