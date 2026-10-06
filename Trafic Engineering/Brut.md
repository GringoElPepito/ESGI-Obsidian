Traffic Engineering
- Strictement associé à MPLS-BGP
- QoS : Quality of service

La QoS : fournit des services spécifiques à certains types de trafics au détriment d'autres types de trafics
Sans QoS : 
- FIFO (First In/First Out) pour les équipement bas de gamme, Premier paquet arrivé, premier paquet servi
- Fair queueing pour les équipements un peu plus haut de gammes, répartission équitable de la bande passante entre tous les trafics.

Les différents de trafics n'ont pas tous les mêmes besoins.
La QoS gère 4 problèmatiques :
- Lack of BandWidth
- Packet Loss (congestion)
- Delays
	- Serialization delays : (lien 1Gbs -> delay 1 ns) Pas vraiment de possibilité d'agir dessus pour réduire le délai mis à part brancher un câble permettant un plus gros débit
	- Propagation delays : temps passé sur le lien, Pas vraiment de possibilité d'agir dessus pour réduire le délai
	- Processing delays : temps passé dans le matériel avant prise de décision et commutation
- Jitter (variation de délais entre les paquets d'une même communication)


Exemple Flux FTP :
BW : +++
Delay : -
Packet Loss : -
Jitter : -

Exemple Flux VoIP :
BW : --
Delay : ++
Packet Loss : ++++
Jitter : ++++

DSP Digital Signal Processor puce de récéption du trafic liés aux appels pour les téléphones

Analogique -> 1 lien = 1 appel
Pour palier les problèmes liés à l'analogique, passage au numérique.

Numérique -> 8 échelles