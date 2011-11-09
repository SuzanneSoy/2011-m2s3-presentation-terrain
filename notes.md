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
- Metaballs : ce sont des isosurfaces. 
Une des premières utilisation répendue des isosurfaces.

- Surface 2D d'un bruit 3D : au lieu d'utiliser voxel utiliser isosurfaces coûte moins cher.

- Simplification de nuages : avec temps de calcul plutôt réduit. Angrandir les faces et mettre une texture floue vers transparent.

- Surface de l'eau : Les raytracer permettent de réaliser ce genre de forme.


Ray casting
-----------
- Très simple à implémenter : il y a un shéma.
- Sampling : problème : Les points éloignés vont beaucoup varier d'une image à l'autre donc scintillement.
Lancer une série de rayons par pixel autour de la cible et faire la moyenne.
Autre solution mip mapping de texture : avec perlin noise il suffit de baisser le nombre d'octaves (résultat plus homogène).
- Très lent.
- Démonstration.
Concours d'assembleur : Le binaire ne fait 512 octets.
- Monte Carlo : Plutôt que de redessiner chaque frame en entier, retracer des pixels aléatoirement rapprochement du sampling (Attracteur de Peter de Jong).



LOD
===
ROAM
----
- Triangle bintree : qu'est-ce que c'est ? constitution.
- split merge : figure split forcé. Ensuite merge du premier triangle.
CLOD : continuous level of detail. Mise à jour incrémentale du mesh pour l'optimiser pour la position de caméra courante.
- Défaut maximal visible à l'écran : borne maximale de l'erreur sur la hauteur du contenu. On mesure la surface projetée à l'écran du pavé triangulaire. C'est l'erreur maximale en pixels.
- Split et merge queue : tri des triangles en fonction de leur erreur en pixel. On découpe ceux qui ont une trop grosse erreur et on fusionne ceux qui ont une erreur faible.
- Frustum culling : utilisation de drapeaux. pour chaque triangle de l'arbre de LOD on marque si il est entièrement visible entièrement caché ou partiellement visible. On recalcule à chaque frame ces drapeaux. Quand on rencontre un entièrement visible qui l'est toujours et pareil pour caché on ne parcourt pas le sous-arbre.
- Améliorations :
  - Triangle stripping : mettre à jour incrémentalement des listes de triangles contigus.
  - Geomorphing : quand on rajoute un vertex on l'anime depuis sa position interpolée.
  - Calcul différé des priorités dans les queues : plutôt que de recalculer l'erreur en pixels de chaque triangle à chaque frame recalculer uniquement ceux qui risquent de se "superposer".
  - Temps réel : on peut arrêter les split merge à tout moment on à un mesh cohérent et de bonne qualité.
- Complexité : O(nombre ...).



FIN.
