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
- 0000AB.123456
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