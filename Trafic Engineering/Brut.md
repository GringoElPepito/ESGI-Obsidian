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

Numérique -> 8 échelles espacés, 16 cordes par échelles espacés.

Pour numériser un signal il faut samplé 2 x plus souvent que l'amplitude observée. donc 8000F/s -> 64kbs
8000 sample par seconde, chacun sample appartient à une corde dans 1 échelle codé sur 8 bits.

Vitesse d'un lien Serial 1,544 Mbs -> 23 canaux à 64kbs

Code : Numériser le flux analogique à la source et le dénumériser à destination
- G.711 : codec natif, non compressif (le principe expliqué au dessus) -> 64kbs. Idéal pour le LAN
- G.729 : codec compressif : 8kbs. Attention pas compatible
- 