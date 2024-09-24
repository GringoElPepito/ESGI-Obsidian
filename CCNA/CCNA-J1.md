r# Définition
Les réseaux
ISO institution international
# Types de réseau
 - LAN : Local Area Network -> portée limitée (réseau de campus), pas de coûts liés à l’utilisation de la connexion (permanente)
 - MAN : Metropolitan Area Network -> town & neighborhood, interconnexion entre les LAN, utilise des technos LAN répétés/ré-amplifiés, jusqu'à 100 km.
 - WAN : Wide Area Network -> pas de limitation géo.
# Model OSI
OSI = Open System Interconnections, crée par l'ISO.
PDU = Protocol Data Unit
Modèle en 7 couches/layers :
- 7 Application : 
	- PDU : DATA
	- GUI (Graphical Unit Interface) Client/Serveur
- 6 Presentation : 
	- PDU : DATA
	- Syntaxe, chiffrement, compression
- 5 Session : 
	- PDU : Transaction / Data
	- dialog control, recovery point
- 4 Transport : 
	- PDU : Segment
	- TCP ou UDP (avant aussi SPX)
	- Segmentation des data, 
	- flow control -> n° port, 
	- reliability (TCP) -> Acknowledgement, Windowing /Sliding windows (nombre de datagram avant envoie d'accusé de réception). 
	- Ajout d'en-tête
	- Matériel : Firewall
- 3 Réseau/Network : 
	- PDU : Paquet / Packet
	- Data Delivery  -> routed/routable protocol -> mécanisme d'adressage hiérarchique.  IPv4 / IPv6 / IPX / AppleTalk
	- Path Selection -> routing protocol Static / RIP / OSPF / EIGRP / IS-IS / BGP
	- Matériel : Router, MLS (Multi Layer Switch)
- 2 Liaison de données : 
	- PDU : Trames / Frame
	- Linear / Flat Addressing
	- Hop by Hop delivery. 
	- S'adapte à l'architecture utilisée sur un segment réseau (ex: passer de Ethernet à xDSL)
	- LAN : Ethernet, Token Ring, FDDI
	- WAN : PPP, RNIS, xDSL, HDLC
	- Matériel : Bridge, Switch
- 1 Physique :
	- PDU : Bits
	- Codage / Décodage du signal électrique, lumineux ou hertzien.
	- Matériel : Media, Repeater, Transceiver, Hub
Les couches 7 à 5 sont plutôt destinés aux développeurs.
Les couches 4 à 1 concernent quant à elles le réseau en lui même

La descente du modèle OSI (envoie de données) correspond à l'encapsulation.
La monté du modèle OSI (réception de données) correspond à la désencapsulation.
## Encapsulation / Désencapulsation
A
P [=&=&=&=&=&=&=&=&=&=&=&=&=&=&=&=&=&=&=] = [data]
S
T [N°| port src | port dest [data]] = [Segment TCP]
N [IP src | IP dest [Segment TCP]] = [Packet IP]
DL [MAC src | MAC dest [Packet IP]] = [Trame Ethernet]
P [Trame Ethernet] -> 01010101010
Phrase mnémotechnique : All People Seem To Need Data Processing

# Model TCP/IP

| OSI          | TCP/IP         |
| ------------ | -------------- |
| Application  | Application    |
| Presentation | Application    |
| Session      | Application    |
| Transport    | Transport      |
| Network      | Internet       |
| DL           | Network Access |
| Physique     | Network Access |
## 1) Couche Physique
- ### Câble : 
	- Copper / Cuivre :
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

## 2) Couche commutation (proche en proche)
2 méthode d'accès au média :
- Deterministic, pour communiquer une station doit capturer le jeton et donc seul cette station peut émettre : 
	- Token Ring
	- FDDI
- Non-determinstic, mode compétitif :
	- Ethernet
Ethernet utilise CSMA/CD : Carrier Sense Multiple Access / Collision Detection
Pour émettre la station écoute le média: 
- si elle reçoit du signal électrique = communication en cours donc attente
- si le média semble libre -> émission.
Il est possible que x stations perçoivent le média comme libre et émettent simultanément -> collision. Il en résulte un signal corrompu qui sera véhiculer sur le réseau suite à la collision.
Les stations ayant générés la collision -> cessent d'émettre et choisissent un timer sur 8 positions. La première station arrivé à 0 réémet. Si les stations avaient le même timer -> nouvelle collision -> timer sur 16 positions, si nouvelle collision -> timer sur 32 positions.

#### Domaine de collision :
Hub interconnecté. Plus le réseau est grand -> plus il y a de collisions.
[<-->] 
|
[<-->]
|   |   |
[<-->]

### En-tête Ethernet

| Préambule | Start delimiter | Mac destination | Mac source | Type/Length | Data             | FCS      |
| --------- | --------------- | --------------- | ---------- | ----------- | ---------------- | -------- |
| 7 octets  | 1 octets        | 6 octets        | 6 octets   | 2 octets    | 46 à 1500 octets | 4 octets |
- Préambule :  négociation de la communication, vitesse, duplex etc... (pas compris dans  la taille de la trame).
- Start delimiter : indique le début de l'envoie de le trame (pas compris dans la taille de la trame).
- Mac Source
- FSC : contient le CRC Champ de Redondance Cyclique -> checksum permettant de vérifier. MTU(Maximum Transfer Unit) : limite de taille de la trame Ethernet par défaut 1518 octets
### Adresse MAC
Codé sur 48 bits en Hexadécimal. séparé en 2 partie 
- 6 premiers chiffres sont l'OUI : Organizational Unique Identifier gérés par IETF (Internet Engineering Task Force). Identifie le constructeur de la carte
- 6 derniers chiffres : gérés par le constructeur n° de série de la carte. 
Notation des adresses MAC : 
- 00-AB-12-EF-23-00
- 00:11:AA:33:EF:89
- 00aaef34dc34
- 00AA.AB12.3456
- 0000AB.1234561er
### 1er cas de figure
[<->] == HUB
[B] == Bridge
[<->(E0)]----[B]----[<->(E1)]
|     |     |
A   B    C

| E0  | A,B,C |
| --- | ----- |
| E1  | D,E,F |

Sur un device L2 (équipement couche 2) il y a une table qui associe les MAC au port connecté -> CAM : Content Addressable Memory 
1. Table CAM remplie
2. A veut communiquer avec B
3. A connaît la MAC de B -> Trame unicast
4. Le Bridge stocke et horodate la MAC source
5. Consulte CAM pour destination -> même côté que la source
6. DROP du paquet.

Le bridge fait office de mur et sépare le domaine de collision en 2, E0 et E1 forment donc chacun un domaine de collision distinct.

Horodatage : compteur de 300 s. avec l'horodatage si une machine se déconnecte du réseau son adresse MAC son enregistrement dans CAM est supprimé au bout des 300 s.

To populate its CAM table the L2 device stores & timestamps the source MAC address


### 2e cas de figure
[<->(E0)]----[B]----[<->(E1)]
|     |     |
A   B    C

1. Table CAM remplie
2. A veut communiquer avec D
3. A connaît la MAC de D -> Trame unicast
4. Stocke & timestamps MAC de A
5. Consulte CAM pour destination -> sur port E1
6. Commute sur E1 si le média est libre (cf. CSMA/CD).

### 3e cas de figure
[<->(E0)]----[B]----[<->(E1)]
|     |     |
A   B    C

| E0  |     |
| --- | --- |
| E1  |     |
1. Table CAM vide (The L2 device has just restarted)
2. A veut communiquer avec B
3. A connaît la MAC de B -> Trame unicast
4. Le bridge stocke et timestamps -> MAC source ajout de la MAC de A sur le port A E0
5. Consulte CAM pour destination -> ne connaît pas la MAC destination
6. laisse passer la trame sur E1
[!!] Quand un dispositif de couche 2 reçoit une trame et ne connaît pas la MAC de destination, il commute cette unicast sur tous les ports sauf le port source => unicast flood

### 4e cas de figure
[<->(E0)]----[B]----[<->(E1)]
|     |     |
A   B    C

1. Table CAM remplie
2. A veut communiquer B
3. A ne connaît pas la MAC de B -> requête ARP (Address Resolution Protocol - Couche 2). -> Broadcast donc MAC destination = FF:FF:FF:FF:FF:FF
4. Stocke et timestamps MAC A
5. Consulte CAM pour destination -> laisse passer le broadcast.
[!!] dispositif de couche 2 ne sait pas (ne doit pas) traiter les broadcasts.

[<-<-->->] = Switch
[<-<-->->] Micro segmentation = 1 station / port de switch.
Dans un switch bande passante dédié par port.

### Mode de commutation

| Préambule | Start delimiter | Mac destination | Mac source | Type/Length | Data             | FCS      |
| --------- | --------------- | --------------- | ---------- | ----------- | ---------------- | -------- |
| 7 octets  | 1 octets        | 6 octets        | 6 octets   | 2 octets    | 46 à 1500 octets | 4 octets |
Store & Forward : Le switch attend de recevoir toute la trame, recalcule le FCS -> si OK commute. Permet d'identifier le nombre d'erreur CRC, si grand nombre très probablement pb câble.
Méthode Cut Through :
- Fragment Free : Le switch récupère la trame jusqu'au champ Type/Longueur et vérifie que la longueur annoncé est supérieur ou égale à 64 octets 
	- -> Si OK commence a commuter
	- -> Si pas OK drop
- Fast Forward -> dés que le switch a reçu la MAC destination il commence à commuter. Sur le bon port s'il connaît l'adresse MAC, sur tous les ports s'il la connaît pas.

## 3) Couche 3 :
- Data Delivery -> adressage hiérarchique IPv4, IPv6.
- Path Selection -> routage
### IP
protocol non orienté connexion -> ne s'assure pas que la connexion est bidirectionnelle.
découpage en-tête :
- Version : indique si IPv4 ou IPv6
- XXX
- Type of Service : 
- Total Length : 
- Fragment Offset :
- Identification :
- TTL (Time To Live) :
- Protocol :
- Header Checksum :
- Source Address :
- Destination Address :
- IP Option
l'en tête IP est de 20 octets.

### IPv4
les adresses IP sont séparés en classes, 32 bits réparties en 4 octets notation en décimal pointés. Il y a 2 parties dans l'adresse IP : Réseau / Host, on distingue ces 2 parties à l'aide du masque -> bit à 1 = Réseau / bit à 0 = Host.

#### Classe A
Pour les très grands réseaux.
Par défaut : 
- Réseau : 1er octet
- Host : 3 derniers octets
Masque : 255.0.0.0 -> /8 bitcount -> on compte le nombre de 1 dans le masque / CIDR = Classless InterDomain Routing.
Le 1er octet est compris entre 1 et 126
Il existe 126 réseaux de classe A chacun comporte 2²⁴ - 2 = 16 777 214 ID
on retire 2 adresses :
- La première : adresse réseau
- La dernière : adresse broadcast
#### Classe B
Pour les réseaux de tailles moyennes
Par défaut : 
- Réseau : 2 premiers octet
- Host : 2 derniers octets
Masque : 255.255.0.0 -> /16
Le 1er octet est compris entre 1 entre 128 et 191
Le second octet entre 0 et 255
Il existe 64 x 256 = 16384 réseaux de classe B
Chacun compte 2¹⁶ - 2 = 65535 IP

#### Classe C
Pour les réseaux de petites tailles
Par défaut : 
- Réseau : 3 premiers octet
- Host : le dernier octets
Masque : 255.255.255.0 -> /24
Le 1er octet est compris entre 1 entre 192 et 223
Le second octet entre 0 et 255
Il existe 64 x 256 x 256 = 2 097 152 réseaux de classe C
Chacun compte 2⁸ - 2 = 254 IP

#### Classe D
Pour le multicast (IP pour envoyer des flux à un groupe d'abonnés)
224.0.0.1 à 239.255.255.255

#### Classe E
Réservé (non utilisable)
240.0.0.0 à 255.255.255.254

#### IP localhost
127.X.Y.Z
ping localhost -> pile TCP/IP est fonctionnelle d'un point de vue protocolaire

### Subnetting
Conversion décimal / binaire
Un nombre est composée de chiffre. et est exprimé dans une base (binaire, hexa...)
Chaque chiffre a une place dans le nombre -> rang
base 10 ex -> 6⁴2³7²1¹3⁰ -> 6 x 10⁴ + 2 x 10³ + 7 x 10² + 1 x 10¹ + 3 x 10⁰

#### Conversion binaire / décimal
1⁷1⁶0⁵1⁴0³1²0¹1⁰ = 1 x 2⁷ + 1 x 2⁶ + 1 x 2⁴ + 1 x 2² + 1 x 2⁰
128 + 64 + 16 + 4 +1 = 213


187 - 128(2⁷) = 59
59 - 32 (2⁵) = 27
27 - 16 (2⁴) = 11
11 - 8 (2³) = 3
3 - 2 (2¹) = 1
1 - 1 (2⁰) = 0


| poids     | 1   | 2   |     |     |     |     |     |     |     |     |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Puissance | 0   | 1   |     |     |     |     |     |     |     |     |


#### Méthode
Soit le réseau 192.168.10.0/24 (255.255.255.0)
on veut des SR d'au moins 25 IP
1. On ajoute 2 au besoin (@Réseau & @Broadcast)
2. Combien de bits pour coder 27 possibilités -> 5 car 2⁵ = 32 ce qui est la valeur binaire égale ou supérieur à 27
3. Masque : on réserve 5 bits à 0 dans la partie classful (vers la gauche) avec des 1
255.255.255.224 = 1111 1111 . 1111 1111 . 1111 1111 . 1110 0000
pour le pas 256 - 224 = 32 -> les SR sont de 32 en 32 sur le 4e octet
IP directed Broadcast

### Routeur
équipement de couche 3 d'interconnexion réseau IP
composants : 
- CPU
- RAM : mémoire de travail, L'OS chargé le fichier de conf active, les tables et les caches files d'attente
- Flash : HD contient l'image de l'OS + d'autres fichier
- ROM : IOS minimaliste. Instruction Bootstrap
- NvRAM : Non volatile Random Access Memory -> fichier de configuration sauvegarder.
- interfaces : port de connexion

IOS Internetwork Operating System

Premier démarrage => Routeur vierge => mode Setup
Configuration initial => obligatoirement par le port console (RS232). => Nécessite accès physique.
Une fois le routeur configuré et accessible par le réseau IP
VTY = Virtual Teletype => SSH, Telnet

lien auxiliaire => on y branche un modem analogique liés à 1 prise téléphonique => associés à un numéro de téléphone => à distance de se connecter au matériel

Configuration -> ligne de commande -> CLI Command Line Interface
A la connexion = mode user => prompt : `router>` | accès limité au commande de visualisation.
Accès au commande pour des tests de base (ping, traceroute).

Mode privilégié -> `router>enable` => `router#` toutes les commandes de visualisation, tests avancés, commandes de dépannage (debug) -> voir en temps réel ce que fait le matériel.

Mode configuration global : `router# configure terminal` => `router(config)#` Modifier "parameters that affect the device as a whole".

Mode de configuration spécifique :
- interface
- protocole routage
- politique QoS
- etc...

Système d'aide :
- Complétion (touche tab) -> termine une directive s'il n'y a pas d’ambiguïté
- auto-complétion -> "raccourcir" les commandes s'il n'y a pas d'ambiguïté
- ? -> directive ? : toutes les options possibles après cette directive ; direc? -> affiche la complétion possible de la commande.

Changer hostname : `hostname NOUVEAU_NOM`

Configurer les mots de passe :
- Console : accéder à la console 
	1. `router(config)#line console 0`
	2. `#password MDP`
	3. `login` <- active l'authentification
- VTY (Telnet,SSH) : accéder à la console à distance
	1. `vty line vty 0 15` <- permet 16 connexions pour administration à distance
	2. `password MDP`
	3. `login`
- MDP privilégié : passer en mode privilégié, il est obligatoire pour accéder au mode privilégié depuis une session distante
	1. `(config)#enable secret MDP` <- mot de passe automatiquement chiffré dans le fichier de conf (MD5).

Password Encryption :
Hormis le mot de passe privilégié, les autres mot de passes ne sont pas chiffrés dans le fichier de conf.
`(config)#service password encryption`

Voir fichier de configuration : `show running-configuration`

Configuration d'une bannière : message s'affichant à la CLI du matériel. ```
```cisco-CLI
(config)#banner motd $ <- escape character
info
info
info$
```

Configurer les interfaces : 
```cisco-CLI
(config)#int type slot/sous-slot/port ex int e0/0
(config-if)#ip address 192.168.10.254 255.255.255.0
(config-if)#desc "LAN Site1"
(config-if)#no shut
(config-if)#int serial 1/0
(config-if)#ip addr 1.1.1.1 255.255.255.252
(config-if)#desc "Lien vers Paris"
(config-if)#no shut
```
