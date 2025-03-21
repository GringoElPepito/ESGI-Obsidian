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
  Pas de DR(Designated Router)/BDR(Backup Designated Router). Les message OSPF (Hello,LSA) sont envoyés sur 224.0.0.5
- Broadcast -> Encapsulation Ethernet. Plusieurs Adjacences
  Election de DR/BDR
- NMBA -> pas vu au CCNA
- Point-to-multipoint -> pas vu au CCNA

Sur un environnement de broadcast :
But du DR ( Designated Router ) : il est garant de la mis à jour de la LSDB (et donc de la table de routage) sur un environnement broadcast. Le but d'avoir un DR est de limiter le nombre d'adjacence sur un réseau où il peut y avoir de nombreux routeurs. Si ce concept n'existait pas il y aurait n(n-1)/2 adjacences où n est le nombre de routeurs.

Le BDR ( Backup DR ) écoute passivement les LSA. Son seul but est de devenir DR si le DR actuel tombe.

Les autres routeurs sont appelés DROTHER

Quand il y a une modif, le routeur envoie la LSA sur 224.0.0.6 -> DR et BDR reçoivent. Le DR vérifie que la LSA est OK et envoie la LSA sur 224.0.0.5 à tous les autres routeurs.

/!\ la notion de DR/BDR n'a rien à voir avec la notion d'aire. La notion de DR/BDR est uniquement lié à la notion de broadcast

### Election DR/BDR
Il n'y a pas préemption sur l'élection, une fois un routeur élue, il n'y a pas de réélection sauf si le DR & BDR tombent
1) Basée sur la priorité OSPF de l'interface connectée à l'environnement broadcast
   Prio : valeur de 0 à 255. Par défaut 1. Si la valeur = 0, le routeur ne pourra jamais devenir DR ou BDR.
   Pour changer la priorité : `ip ospf priority VALEUR`
   Si impossible de se départager sur la priorité OSPF, l'élection sera basée sur le ROUTER-ID OSPF
2) Chaque Routeur OSPF a un router-id (codée sur 32 bits : format d'une IPv4) unique dans tout l'AS. Le routeur qui a le router-id le plus élevé devient le DR
   Pour trouver son router-id, le router regarde
   - la commande `router-id` => `(config-router)# router-id w.x.y.z`
   - Si cette commande n'a pas été configurée, le router-id sera :
	   - L'IP la plus élevée parmi les interfaces de loopback (logical, virtual)
	   - S'il n'y a pas de d'interface de loopback alors L’IP la plus élevée parmi toutes les interfaces du routeur sera choisi pour être le router-id.
Voici un tableau de voisinage
```
Neighbor ID     Pri   State           Dead Time   Address         Interface
192.168.2.254   255   FULL/DROTHER    00:00:33    11.1.1.2        Ethernet0/1
192.168.3.254   254   FULL/DROTHER    00:00:35    11.1.1.3        Ethernet0/1
192.168.4.254     1   FULL/BDR        00:00:31    11.1.1.4        Ethernet0/1
```
Pour forcer une réélection :
```
clear ip ospf process
```