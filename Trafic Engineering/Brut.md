Traffic Engineering
- Strictement associé à MPLS-BGP
- QoS : Quality of service

La QoS : fournit des services spécifiques à certains types de trafics au détriment d'autres types de trafics.
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

La QoS s'applique aux paquets sortants, il va prioriser

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
- G.729 : codec compressif : 8kbs. Attention pas compatible avec la polyphonie (Plusieurs sources sonores, typiquement MoH : Music on Hold)


Paquetisation de la VoIP :
En général, chaque paquet contient 20ms de voix, sur de la vidéo conférence on peut retrouver 40 à 80ms de voix.

Taille en octet 
Un paquet = 20ms -> 1/50 d'1 seconde

Pour G.711 -> 64000 / 50 / 8 = 160 octets
Pour G.729 -> 8000 / 50 / 8 = 20 octets

RTP -> Real Time Protocol, sert à gérer le réordonnancement des paquets de voix qui n'est pas traité par UDP

Taille des headers :
Ethernet : 18 octets
IP : 20 à 60x octets 
RTP : 12 octets
UDP : 8 octets

Structure du paquet :
| L2 | IP | RTP | UDP | Payload 20ms |

POTS : Plain Old Telephony System/Service

BP réelle = (Taille totale de la trame en oct X BP nominale du codec en kbs) /Taille du payload en oct

Bande passante réelle utilisé par un appel utilisant le codec G.729
((18+20+12+8+20) x 8)/20 = 31,2 kbs 

Donc pour 10 appels, il faudra configuré la QoS du trafic temps réel/VoIP avec une bande passante maximale de 312kbs.

CAC : Call Admission Control -> limitation du nombre d'appel sur l'IPBX.

SNMP permet de supervisé 
SNMPv1 est totalement obsolète
SNMPv2 n'est pas chiffré donc préféré l'utilisation de SNMPv3 qui lui est chiffré.

Les pirates tentent dans un premier temps de récupérer de l'information, 

Protocole utilisé
UDP 161 (port serveur - machine sur laquelle on récupère les informations)
UDP 162 (port client - Machine de supervision qui va récupérer les infos (PRTG, Nagios, Zabbix))

Les composants SNMP
- La MIB (Management Information Base)
	- Collection d'objets gérés (Managed Objects)

La MIB
- La MIB est structuré par SMI (Simple Management Information)
- SNMP ne définit pas lui-même quelles informations un système peut exploiter
- 