Cette couche sert principal à déterminer le chemin que doivent prendre les données (pour cette couche on parlera de paquet IP) pour atteindre leur destination. Ceci est rendu possible grâce à l'adressage hiérarchique proposé par [[IPv4]] & [[IPv6]] (on appelle ça le data delivery). Pour choisir le meilleur chemin possible (Path Selection), il existe différent type de routage :
- Static
- RIP
- OSPF
- etc...
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