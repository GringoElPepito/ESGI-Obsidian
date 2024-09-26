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
- Table du voisinage -> liste tous les routeurs EIGRP directement -> accélère la convergence et la fiabilité. est maintenu par l'envoi de données sur l'addresse 224.0.0.10
- Table Topologique -> tous les chemins possibles pour chaque destination.
- Table de routage -> la ou les meilleurs (si métrique égal) pour chaque destination.
EIGRP envoie un message "Hello" toutes les 5 secondes pour vérifier que les voisins ne sont pas HS si 3 "Hello" sans réponse considère que le voisin est HS.

Métrique : basées sur 4 facteurs : 
- Bande passante (BW) des liens en kbps
- Delay des liens traversé pour arrivé à destination
- La charge
- La fiabilité valeur débutant à 255/255 (baisse si erreur dans les paquets et remonte si  pas d'erreur pendant un certain temps)
- MTU
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