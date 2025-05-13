- ### Câble : 
	- Copper / Cuivre :
		- Serial, T1 Numeris -> 1,544Mbps
		- Coaxial : 
			- 10 Base 2, Thinnet, 185 m, 10 Mbps
			- 10 Base 5, Thinnet, 500 m, 10 Mbps
		- Twisted Pair : 4 pair de fils torsadés.
			- UTP : Unshielded Twisted Pair-> entre Station de travail et prise murale
			- STP : Shielded Twisted Pair -> une ou plusieurs couches de blindage pour isoler des interférences.
	- Fiber / Fibre :
		- Multimode :
			- core (en verre/Silice) entre 50 & 62,5 micro mètre de diamètre
			- Enveloppe (en verre/Silice sans réflexion) 125 micromètre de diamètre
			- Kevlar (Blindage)
			- Plastique
			- -------------------
			- LED envoie signal non orienté => le signal rebondit sur les bords du core (indice de réflexion signal utile & indice de réfraction perte du signal).
			- Distance max 550 m
		- Monomode :
			- core (en verre/Silice) de 8,3 micro mètre
			- Enveloppe (en verre/Silice sans réflexion) 125 micromètre de diamètre
			- Kevlar (Blindage)
			- Plastique
			- ------------------
			- LASER envoie signal orienté => réduction de l'indice de réflexion. 
			- 40 km de distance sans répéteur.
---------------

- ### Répéteur [->]
Réamplifie et resynchronise le signal pour étendre la porté
--------------------

- ### Hub [<-->]
Répéteur multiport, réamplifie et resynchronise le signal et le renvoie sur tous les ports, bande passante partagé.
- Topologie physique (ce que l'on voit) : star
- Topologie logique (comment le signal est géré) : bus
-------------------

- ### Transceiver
permet de modifier le type de câblage et de codage. ex: module SFP, GBIC
--------------

- ### Influence sur le signal :
	- Atténuation -> tous les câbles, la résistance du matériel de support fait faiblir le signal
	- Propagation -> tous les câble, le signal va s'allonger dans le temps ce qui peut perturber sa lecture
	- Diaphony -> (copper), lorsque le cuivre est traversé par un courant, un champ électromagnétique se génère, ce champ peut perturbé le signal d'un autre câble.  Solution -> Shielded.
	- Paradiaphony -> (copper : twisted), diaphony entre les fils d'un même câble.
Câble Droit (Straight-through)
EIA-TIA 568B, de la gauche vers la droite, tête vers vous (téton vers le bas) :

| Blanc/Orange |     |     | Blanc/Orange |
| ------------ | --- | --- | ------------ |
| Orange       |     |     | Orange       |
| Blanc/Vert   |     |     | Blanc/Vert   |
| Bleu         |     |     | Bleu         |
| Blanc/Bleu   |     |     | Blanc/Bleu   |
| Vert         |     |     | Vert         |
| Blanc/Marron |     |     | Blanc/Marron |
| Marron       |     |     | Marron       |

Si câble droit 2 côtés identiques
Câble Croisé (Crossover)
1 côté sera comme le câble droit et l'autre sera :

| Blanc/Orange |     |     | Blanc/Vert   |
| ------------ | --- | --- | ------------ |
| Orange       |     |     | Vert         |
| Blanc/Vert   |     |     | Blanc/Orange |
| Bleu         |     |     | Bleu         |
| Blanc/Bleu   |     |     | Blanc/Bleu   |
| Vert         |     |     | Orange       |
| Blanc/Marron |     |     | Blanc/Marron |
| Marron       |     |     | Marron       |

En respectant ce code couleur, on s'assure qu on croise une paire TX (Transmission) avec une paire RX (Réception) ce qui permet d'annuler les champs électromagnétique, ceci évitant la paradiaphony.