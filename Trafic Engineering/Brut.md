Traffic Engineering
- Strictement associé à MPLS-BGP ou VxLAN -> MPLS-BGP-TE
- QoS : Quality of service

La QoS : fournit des services spécifiques à certains types de trafics au détriment d'autres types de trafics.
Sans QoS : 
- FIFO (First In/First Out) pour les équipement bas de gamme, Premier paquet arrivé, premier paquet servi
- Fair queueing pour les équipements un peu plus haut de gammes, répartition équitable de la bande passante entre tous les trafics.

Les différents trafics n'ont pas tous les mêmes besoins.
La QoS gère 4 problèmatiques :
- Lack of BandWidth
- Packet Loss (congestion)
- Delays
	- Serialization delays : (lien 1Gbs -> delay 1 ns) Pas vraiment de possibilité d'agir dessus pour réduire le délai mis à part brancher un câble permettant un plus gros débit
	- Propagation delays : temps passé sur le lien, Pas vraiment de possibilité d'agir dessus pour réduire le délai
	- Processing delays : temps passé dans le matériel avant prise de décision et commutation
	- Queueing delay : Temps passé par le paquet à attendre dans la file d'attente d'une interface.
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

DSP Digital Signal Processor puce de réception du trafic liés aux appels pour les téléphones

Analogique -> 1 lien = 1 appel
Pour palier les problèmes liés à l'analogique, passage au numérique.

Numérique -> 8 échelles espacés, 16 cordes par échelles espacés.

Pour numériser un signal il faut samplé 2 x plus souvent que l'amplitude observée. donc 8000F/s -> 64kbs
8000 sample par seconde, chacun sample appartient à une corde dans 1 échelle codé sur 8 bits.

Vitesse d'un lien Serial 1,544 Mbs -> 23 canaux à 64kbs

Codec : Numériser le flux analogique à la source et le dénumériser à destination
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

BP réelle = (Taille totale de la trame en oct X BP nominale du codec en kbs) / Taille du payload en oct

Bande passante réelle utilisé par un appel utilisant le codec G.729
((18+20+12+8+20) x 8)/20 = 31,2 kbs 

Bande passante réelle utilisé par un appel utilisant le codec G.711
((18+20+12+8+160) x 64) / 160 = 87,2 kbs

Donc pour 10 appels, il faudra configuré la QoS du trafic temps réel/VoIP avec une bande passante maximale de 312kbs.

CAC : Call Admission Control -> limitation du nombre d'appel sur l'IPBX.

SNMP permet de supervisé 
SNMPv1 est totalement obsolète
SNMPv2 n'est pas chiffré donc préféré l'utilisation de SNMPv3 qui lui est chiffré.

Les pirates tentent dans un premier temps de récupérer de l'information, 

Ports utilisés
UDP 161 (port serveur - machine sur laquelle on récupère les informations)
UDP 162 (port client - Machine de supervision qui va récupérer les infos (PRTG, Nagios, Zabbix))

Les composants SNMP
- La MIB (Management Information Base)
	- Collection d'objets gérés (Managed Objects)

La MIB (Management Informations Bases)
- La MIB est structuré par SMI (Simple Management Information)
- SNMP ne définit pas lui-même quelles informations un système peut exploiter
	- SNMP utilise une structure extensible dans laquelle les informations disponibles sont définies dans des MIBs
	- Les MIB décrivent la structure des données collectés pour un dispositif géré
		- Elles utilisent des espaces de noms (namespace) contenant des identifiants d'objets (OID -> Object Identifiant)
		- Chaque OID identifie une variable qui peut être lue ou modifié par SNMP

La MIB se présente sous la forme d'un objet à propriété comme on pourrait le retrouver en programmation.
Exemple MIB :
0. CCITT
1. ISO
	0. standard
	1. registration authority
	2. member body
	3. organization
		6. DoD
			1. Internet
				1. directory
				2. management
					1. MIB-2
				3. experimental
				4. private
					1. enterprises
				5. security
				6. SNMPv2
				7. mail
2. ISO - CCITT

Les informations se retrouve généralement au niveau des propriétés de MIB-2. L'OID de MIB-2 est 1.3.6.1.2.1


Config SNMP
```
snmp-server host 192.168.10.100 version 2 toto
snmp-server community toto ro
snmp-server contact Admin ISTRATEUR
snmp-server location SITE1-PARIS
```


Netflow -> Implémentation Cisco
Netflow est un protocole réseau développé par Cisco pour collecter, mesurer et analyser le trafic de données IP qui transite par un routeur ou un commutateur.
Standard IPFX permet de récupérer les 
Netflow v9


Configuration 
```
ip flow-export vers 9
ip flow-export destination 192.168.10.100 50000
ip flow-cache timeout active 1 /// en minutes
ip flow-cache timeout inactive 30 /// en secondes
int g0/1
ip flow ingress
ip flow egress
exit
do wr
```


IPSLA Simulation de flux aller-retour pour tester la QoS
Config SITE 1
```
ip sla 1
udp-jitter 2.2.2.1 40000 codec g711a codec-inter 10 codec-num 500 codec-size 160
tos 46
exit
ip sla schedule 1 life forever start now !!! Déclenche la simulation de trafic immédiatement et de manière permanente
```

Config SITE2
```
ip sla responder
```

Pour de la téléphonie tant que l'aller-retour prend moins de 200ms il n'y a pas de problème au niveau de la communication, jusqu'à 400ms, un délai peut être ressenti, au-delà la communication n'est plus fluide.
Min Opinion Score est une note permettant d'évaluer la qualité du service elle est comprise entre 1 et 5.

Marking/Coloring
Après reconnaissance de trafic -> marquage des paquets (ou des trames Ethernet) pour classer les flux


FTP actif port 20 sert au contrôle et port 21 à la data.
FTP passif port aléatoire.

QoS sur la sortie WAN ou sur les ports Trunk pouvant être des goulot d'étranglements.
CoS Class of Service
Si on souhaite marquer une trame Ethernet, il faut que celle-ci soit tagué via du dot1q qui ajoute une en-tête supplémentaire contenant un champ CoS.
Reconnaissance :
- On pourrait imaginer vérification des n° de ports, des IP source ou destination. Pas forcément prévisible et peut changer sur le trajet donc à éviter.
- Pour permettre au matériel d'identifier un trafic, on va activer NBAR -> Network Base Application Recognition. Utilise un fichier PDLM (Packet Description Language Module) (équivalent base virale)

Proxy SSL/TLS va servir de mandataire pour transmettre les requêtes de ces clients aux serveurs Web sur Internet. Le client contacte le Proxy en chiffrant les échanges avec le certificat du Proxy. Le Proxy va recevoir les requêtes, les déchiffrer et regarder leurs contenus puis les transmettre au serveur web cible et inspecter les réponses de ce dernier.

Certain matériel ou application permettent de marquer nativement les paquets. Si les flux ne sont pas marqués nativement -> les reconnaîtres au plus proche de leur génération, de les identifier grâce NBAR et les marquer à ce moment-là.

Classification au niveau IP : Champ ToS (Type of Service : 8 bits) qui contient 2 valeurs la première valeur (IPP : IP Precedence : 3 bits de poids fort du champ les plus à gauche) & (DSCP : Differenciated Service Code Point -> 6 bits de poids forts champs ToS).

Champ TOS :
Dans le cas d'un paquet VoIP

| IPP | IPP | IPP | D   | T   | R   | M   | 0   |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1   | 0   | 1   | 1   | 1   | 0   | 0   | 0   |

- D -> Delay, est-ce que le délai doit être le plus court possible ?
- T -> ThroughPut, est-ce qu'il faut un débit minimum garanti ?
- R -> Reliability, est-ce que la perte de paquet est important ?

Pour le calcul de la valeur on ne prend en compte que les 6 bits avec les poids les plus forts (donc les plus à gauche) ce qui est égale à 46.

Action QoS :
- Strict Priority -> pour une seule classe uniquement (ToIP, Temps réel) : BP maximale mais strictement prioritaire. Permet de transmettre directement les paquets ciblés par cette règle en laissant les autres paquets en tampons.
- CBWFQ : Class-Based Weighted Fair Queueing : Fournir une BP minimale et plus si possible.
- CB Policing : Class-Based Policing : Fournit une BP maximale avec soit remarking/recoloring soit drop pour les flux en excès.
- CB Shaping : Class-Based Shaping : Fournit une BP moyenne mais Attention cela peut générer des délais très importants (voir des timeout). Cela permet de lisser la bande passante, il vide les files d'attentes lorsque le débit réel est inférieur au débit cible et les remplits dans le cas contraire.
- CBWRED : Class-Based Weighted Random Early Detection : Indiquer la probabilité de drop (drop discriminator) selon le type de trafic --> les seuils (modifiables) sont basés sur le champ ToS (IPP ou DSCP)
- Compression des flux ToIP (cRTP : Compressed RTP) -> passer de 40 octets (compression des headers IP+UDP+RTP) à 2 ou 4 octets (si vérification checksum) -> ajoute des délais : à faire sur les liens dont la bande passante est <= T1 (1,544Mbs).

Si congestion sur un équipement, par défaut, l'équipement va drop des paquets de manière aléatoire.

CB Policing permet de définir 3 seuils :
- Max : 1Mbs
- Exceed Traffic, il est possible d'exécuter une action soit drop soit remarking pour déprioriser ce trafic excédentaire.
- Violating Traffic (TAIL DROP), dès qu'un trafic ciblé par le CB Policing tente de dépasser ce seuil celui-ci est automatiquement drop


Champ ToS Per-Hop Behaviour basé sur 3 bits 
- 000 = Défaut (Best effort)
- 101 = Expedited Forwarding
- 001, 010, 011, 100 = Assured Forwarding

Classe 1,2,3 ou 4 en fonction des premiers 

EF Expedited Forwarding
AF Assured Forwarding
AFXY 
- X correspond à la valeur constitué par les 3 premiers bits
- Y correspond à la valeur constitué par les 2 bits suivants

Correspondance :

| Probabilité de suppression | Classe 1                  | Classe 2                  | Classe 3                  | Classe 4 |
| -------------------------- | ------------------------- | ------------------------- | ------------------------- | -------- |
| Faible                     | 001010<br>AF11<br>DSCP 10 | 010010<br>AF21<br>DSCP 18 | 011010<br>AF31<br>DSCP 26 |          |
| Moyenne                    | 001100<br>AF12<br>DSCP 12 | 010100<br>AF22<br>DSCP 20 | 011100<br>AF              |          |
| Importante                 | 001110<br>AF13<br>DSCP 14 | 010110<br>AF23<br>DSCP 22 |                           |          |


QoS : int-serv (RSVP protocole client-serveur permettant de réserver une allocation stricte de la bande passante entre 2 terminaux) et diffserv (Diff).

Pour l'int-serv le client est l'hôte à l'origine de la communication et le serveur est la passerelle de cet hôte

Côté opérateur, celui-ci va généralement faire du in-serve pour garantir la bande passante d'un client à un autre puis du diff-serve pour différencier les classes et mener certaines actions.

Au sein d'un site on va plutôt utilisé de la QoS diff-serve.

SD-WAN correspond à une passrelle ayant plusieurs connnexions ex (Internet avec IPSec, Radio type starlink, MPLS-BGP).

SIP est un proctole de signalisation, il ne transporte pas la voix, il est donc peut groumand en bande passante mais souhaite une latence faible

Classe de service :
- Modèle à 4 ou 5 classes
	- Temps réel
	- Signalisation (Peut être associé à la classe critique si modèle 4 classes)
	- Critique
	- Best Effort
	- Scavenger
- Modèle à 8 classes
	- Voix (Temps réel)
	- Vidéo (Temps réel)
	- signalisation
	- Network Control (Critique)
	- Données Critiques (Critique)
	- Bulk Data (Critique)
	- Best Effort
	- Scavenger
- Modèle à 11 classes
	- Voix
	- interactive-video (video) Conférence Teams
	- streaming-video (video) Netflix
	- signalisation
	- ip routing (Network Control)


Recherche RTP, cRTP, sRTP, RTCP
- RTP Real-Time Protocole -> Permet les échanges de voix/vidéo (Flux Temps réel) et horodatage (timestamp) et réordonnacement des segments via 2 canaux unidirectionnels. Flux continu de données
- cRTP Compressed Real-Time Protocole -> Compresse les en-têtes IP, RTP et UDP de 40 à 2 ou 4 octets (4 si checksums)
- sRTP Secured Real-Time Protocole -> Ajoute le chiffrement et l'intégrité au protocle RTP. chiffrement du payload voip (nécessité que les 2 téléphones aient le même certificat racine) sRTCP permet de chiffrer les échanges de contrôle.
- RTCP Real-Time Control Procole -> Permet de faire un retour sur la qualité de transmission (statistiques liés à la QoS), transmission de métadonnées et de métrique mais pas de voix ou de données utiles, 1 canal bi-directionnel. Flux périodique environ toutes les 5 secondes. Si configuré peut permettre de changer le codec pour un codec moins gourmand en bande passante si problème sur le réseau (Congestion, latence ou autre)

Le marquage ne doit être fait que pour les flux donc l'applicatif ne permet pas le marquage

Configuration :
```
!!! Reconnaissance -> activer NBAR
en
conf t
int gi0/1 !!! Interface WAN
ip nbar protocol-discovery
!!! classification
class-map CM_VoIP
match dscp 46 !!! Si marquage
class-map CM_FTP
match protocol ftp !!! Si pas de ToS fournit par l'applicatif

!!! Création des politiques
policy-map PM_g0/1
class CM_VoIP
priority 312 !!! -> Strict Priority valeur en kbs
class CM_FTP 
bandwith 2000 !!! -> CBWFQ valeur en kbs
set dscp 31 !!! Action de Marking
random-detect !!! CBWRED
class CM_PTP
```