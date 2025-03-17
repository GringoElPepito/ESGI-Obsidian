Cette couche sert principal à déterminer le chemin que doivent prendre les données (pour cette couche on parlera de paquet IP) pour atteindre leur destination. Ceci est rendu possible grâce à l'adressage hiérarchique proposé par [[IPv4]] & [[IPv6]] (on appelle ça le data delivery). Pour choisir le meilleur chemin possible (Path Selection), il existe différent type de routage :
- Static
- RIP
- OSPF
- etc...
Equipement de couche 3 : [[Router]]
### Paquet IP
protocol non orienté connexion -> ne s'assure pas que la connexion est bidirectionnelle.
découpage en-tête :
- Version : indique si IPv4 ou IPv6
- XXX :
- Type of Service : 
- Total Length : 
- Fragment Offset :
- Identification :
- TTL (Time To Live) :
- Protocol :
- Header Checksum :
- Source Address :
- Destination Address :
- IP Option :
l'en tête IP est de 20 octets.

### Le routage :
- Statique -> l'administrateur indique manuellement l'IP du prochain saut pour chaque destination.
- Dynamique -> les routeurs utilisent un protocol (RIP,OSPF) de routage, ces protocoles permettent aux routeurs de s'échanger les réseaux auxquelles ils donnent accès. Le chemin sélectionner sera décidé par la qualité des chemins pour accéder à ces réseaux. S'il existe plusieurs chemin la meilleur route sera enregistré dans la table de routage. Si ce meilleur chemin est HS alors le protocole tentera de déterminer un second chemin possible s'il existe.
## RIP : Routing Info. Protocol
protocole de routage à vecteur de distance -> RIP ne fonctionne qu'avec la table de routage (RIB : Routing Info. Base) -> la meilleur route pour chaque dest. éventuelles les meilleurs si qualité (métrique) égales -> load balancing
Métrique : Hop count -> nombre de routeur traversés jusqu'à destination, max 15 router
Distance administrative : 120
Fonctionnement de RIP :
A---[R1]--O-[R2]-P--[R3]---D 
     |             |            |             B
Distance Administrative (AD) : valeur décimal qui représente la confiance que le router va porter en la source de l'information.
Plus la valeur est faible, plus le router fait confiance et choisi la route. Convergence lente



Delay de serialisation, le temps où la pulsion doit être à 1 ou à 0 pour être compréhensible.
1 nanosecondes pour lien Ethernet 10Gpbs.

## EIGRP : Enhanced Interior Gateway Routing Protocol
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

## OSPF
OSPFv2 pour IPv4 & OSPFv3 pour IPv6
Choisi le chemin avec le plus petit cumul des coûts, basé sur l'algorithme de Dijkstra
Chez Cisco le coût est calculé automatiquement en se basant sur la bande passante ((10^8)/Bande passante) par défaut :
- 1 Mb : 100
- 1,544 Mbs : 64
- 10 Mbs : 10
- 100 Mbs : 1
- 1 Gbs  : 1 !
- 10 Gbs : 1 !
Pour gérer les liens supérieurs à 100Mbs on change soit la bande passante de référence. Soit (le plus utilisé pour l'hétérogénéité) on précise le coût pour chaque interface.
Pour les autres constructeurs, il faut préciser à la main le coût de chaque interface.
Quand un routeur reçoit une maj OSPF ajoute le coût de son interface au coût reçu.

Les area sous OSPF permettent de découper un AS en différentes zones.
OSPF : two level hierarchy design
Il y a deux niveaux de zones, l'area 0 qui est le backbone et toutes les autres zones qui sont connectés a l'area 0. Généralement, la segmentation en aire est géographique
```cisco
R2(config)#router ospf 1
R2(config-router)#netw 192.168.2.0 0.0.0.255 area 0
R2(config-router)#netw 1.1.1.0 0.0.0.3 area 0
R2(config-router)#netw 1.1.1.4 0.0.0.3 area 2
R2(config-router)#netw 11.1.1.0 0.0.0.7 area 1
```
le 1 correspond au numéro de processus

```cisco
O IA     1.1.1.0 [110/74] via 11.1.1.2, 00:17:51, Ethernet0/1
O IA     1.1.1.4 [110/74] via 11.1.1.2, 00:17:51, Ethernet0/1
O IA     1.1.1.8 [110/138] via 11.1.1.2, 00:17:51, Ethernet0/1
O        1.1.1.12 [110/74] via 11.1.1.4, 00:17:51, Ethernet0/1
O IA     1.1.1.28 [110/138] via 11.1.1.4, 00:17:51, Ethernet0/1
                  [110/138] via 11.1.1.2, 00:17:51, Ethernet0/1
O IA     1.1.1.32 [110/202] via 11.1.1.4, 00:17:51, Ethernet0/1
                  [110/202] via 11.1.1.2, 00:17:51, Ethernet0/1
      11.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        11.1.1.0/29 is directly connected, Ethernet0/1
L        11.1.1.5/32 is directly connected, Ethernet0/1
      100.0.0.0/24 is subnetted, 2 subnets
O E2     100.100.100.0 [110/20] via 11.1.1.4, 00:17:51, Ethernet0/1
O E2     100.100.101.0 [110/20] via 11.1.1.4, 00:17:51, Ethernet0/1
O IA  192.168.1.0/24 [110/84] via 11.1.1.4, 00:17:51, Ethernet0/1
                     [110/84] via 11.1.1.2, 00:17:51, Ethernet0/1
O IA  192.168.2.0/24 [110/20] via 11.1.1.2, 00:17:51, Ethernet0/1
O     192.168.3.0/24 [110/20] via 11.1.1.3, 00:17:51, Ethernet0/1
O     192.168.4.0/24 [110/20] via 11.1.1.4, 00:17:51, Ethernet0/1
      192.168.5.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.5.0/24 is directly connected, Ethernet0/0
L        192.168.5.254/32 is directly connected, Ethernet0/0
O IA  192.168.6.0/24 [110/84] via 11.1.1.2, 00:17:51, Ethernet0/1
O IA  192.168.7.0/24 [110/148] via 11.1.1.2, 00:17:51, Ethernet0/1
O IA  192.168.8.0/24 [110/148] via 11.1.1.4, 00:17:51, Ethernet0/1
                     [110/148] via 11.1.1.2, 00:17:51, Ethernet0/1
O IA  192.168.9.0/24 [110/212] via 11.1.1.4, 00:17:51, Ethernet0/1
                     [110/212] via 11.1.1.2, 00:17:51, Ethernet0/1

```
- `O` signifie que la route est au sein de l'aire ou d'une erreur dans lequel le routeur est présent utilisé en priorité par rapport à `O `
- `O AI` signifie que la route passe par une autre aire à laquelle le routeur n'est pas directement connecté (passe généralement par l'area 0)
- `O E2` type par défaut des routes redistribué, dans ce cas la métrique est toujours à 20 car le cumul des coûts n'est pas pris en compte. Si on redistribue en E1, le coût jusqu'à l'ASBR est pris en compte.

ABR : Area Border Router => Connecte l'aire 0 à une ou plusieurs autres aires (R1 et R2 dans notre lab)
Internal Router : toutes les interfaces dans la même aire (mais pas l'aire 0)
ASBR : Autonomous System Boundary Router : router qui connecte notre AS à un ou plusieurs autres AS -> router qui redistribue des routes (R1 dans notre lab)
Backbone : connaît toutes les routes (O, O IA, O E2 ou O E1)
Ordinary : comme backbone mais l'aire 0 (O, O IA, O E2, 0 E1)
Stub : dans une aire Stub, le ou les ABR remplacent les routes externes (redistribuées) par une route par défaut avec comme métrique (l'area default-cost) = 1. (O, O IA, O\*IA). En général : une aire qui n'a pas besoin (ou qui ne peut pas avoir) les routes externes mais qui a plusieurs chemins vers le backbone est une bonne candidate à être Stub
Pour passer un router en stub :
```cisco
en
conf t
router ospf 1
area 1 stub
do wr
```
Totally Stub : dans une aire Totally Stub, le ou les ABR remplacent les routes externes (redistribuées) et les routes par défaut des autres aires par une route par défaut avec comme métrique (l'area default-cost) = 1. (O, O\*IA). En général : une aire qui a pas besoin (ou qui ne peut avoir) ni les routes externes ni les routes des autres aires et qui a un seul chemin vers le backbone est une bonne candidate à être totally stub
Pour passer un router en totally stub :
```
en
conf t
router ospf 1
area 2 stub no-summary
do wr
```

OSPF utilise 3 tables :
- Neighbor Table : liste de tous les routeurs OSPF directement connectés (avec qui l'on partage un réseau). Pour voir cette table => `show ip ospf neighbor`
- LSDB : Link State Database => contient des LSA (Link State Advertisement). Pour voir cette table => `show ip ospf database`
- Table de routage. Pour voir cette table => `show ip route`

Les types de réseaux OSPF :
- Point-to-point -> encapsulation HDLC, PPP, sous-interface point-to-point ATM ou Frame-Relay. Une seule relation de voisinage (adjacency).
  Pas de DR/BDR. Les message OSPF (Hello,LSA) sont envoyés sur 224.0.0.5
- Broadcast -> Encapsulation Ethernet. Plusieurs Adjacences
  Election de DR/BDR
- NMBA -> pas vu au CCNA
- Point-to-multipoint -> pas vu au CCNA
## IS-IS
Intermediate System - Intermediate System, protocole de communication entre routeurs non dépendant du protocole réseau (IPv4,IPv6,TokenRing etc..), possède un fonctionnement très proche de OSPF -> gère la diffusion des routes d'une aire à une autre.