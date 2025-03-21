# EIGRP : Enhanced Interior Gateway Routing Protocol
Successeur d'IGRP (propriétaire cisco). AD interne (générés par EIGRP)  = 90
AD externe (redistribuées dans EIGRP) = 170

EIGRP fonctionne avec 3 tables :
- Table du voisinage -> liste tous les routeurs EIGRP directement connecté-> accélère la convergence et la fiabilité. est maintenu par l'envoi de données sur l'addresse 224.0.0.10
- Table Topologique -> tous les chemins possibles pour chaque destination.
- Table de routage -> la ou les meilleurs (si métrique égal) pour chaque destination.
EIGRP envoie un message "Hello" toutes les 5 secondes pour vérifier que les voisins ne sont pas HS si 3 "Hello" sans réponse considère que le voisin est HS.

Métrique : basées sur 4 facteurs : 
- Bande passante (BW) des liens en kbps
- Delay des liens traversé pour arrivé à destination
- La charge
- La fiabilité valeur débutant à 255/255 (baisse si erreur dans les paquets et remonte si  pas d'erreur pendant un certain temps)
Giving the default constant values, only BW, Delay are used to compute metric.
M = 256(10⁷/BW min sur le chemin + S(delay lien traversé)/10)

Le numéro d'AS

configuration EIGRP : 
```cfg
(config)#router EIGRP 100 <- valeur arbitraire mais même valeur sur tous les routeurs de l'AS
(config)#network 192.168.80.0 0.0.0.255 <- le Masque est inversé
(config)#network 192.168.81.0 <- masque pas obligatoire vu que pas de subnetting
(config)#network 1.1.1.1 0.0.0.3
(config)#network 1.1.1.12 0.0.0.3
(config)#network 1.1.1.16 0.0.0.3
```

Voir voisin eigrp
```
sh ip eig nei
H   Address                 Interface              Hold Uptime   SRTT   RTO  Q  Seq
                                                   (sec)         (ms)       Cnt Num
1   1.1.1.5                 Se1/1                    13 00:15:47   14   100  0  7
0   1.1.1.10                Se1/0                    12 00:15:47   15   100  0  10
```
Hold Uptime : temps écoulé depuis la définition en tant que voisin.
SRTT : temps moyen d'aller-retour.
RTO :
Q count : nb paquet en attente vers ce voisin

voir table topologique, pour chaque destination a un code donnant l'état de la route, réseau de destination, nb successeurs (Next Hop), valeur de la FD(Feasible Distance, meilleur métrique jusqu'à destination).
FC = Feasible Condition (RD<FD)
- Successor : La métrique totale en passant par lui est la + faisable -> c'est la FD.
- Feasible successor : la métrique totale en passant par lui est  > à la FD mail il valide la FC (Feasible Condition (RD<FD))
- Possibility permet de joindre la destination mais il ne valide pas la FC. Si le lien avec le Successor tombe et qu'il n'y a pas de Fesible Successor on va alors requêter son voisin pour demander si sa route est toujours OP. Pendant le requêtage, la route devient active au sein de la table topologique.
  Quand tout est ok, la route est masqué P -> passive.


Etat route :
- A : active
- SIA : Stuck In Active
- P : Passive

EIGRP est un protocol trigger active, qui se met à jour via des déclencheurs.

puis liste des voisins permettant d'atteindre cette destination :
- Via IP du voisin (Métrique totale jusqu'à destination / RD (Reported Distance) -> FD du voisin)
```
sh ip eig topology ('all links' pour les liens en Possibility)
```
![[Pasted image 20240926145100.png]]
Lien série : BW = 1,544mbs delay = 20000


| Code | Type          | AD  |
| ---- | ------------- | --- |
| C    | connected     | 0   |
| S    | static        | 1   |
| D    | EIGRP         | 90  |
| O    | OSPF          | 110 |
| I    | IS-IS         | 115 |
| R    | RIP           | 120 |
| D EX | EIGRP externe | 170 |
Multicast : envoie un flux à un groupe d'abonnés.

[Streamer]==>[RDV]==>[DSLAM opérateur]

Chaque chaine télé à sa propre addresse de diffusion au dessus de 224
PIM : Protocol Independant Multicast, qui permet

RIPv1 envoie MAJ en Broadcast, même les routeurs non-concernés recevra aussi les MAJ.

SI équipement ne gérant pas le multicast 

