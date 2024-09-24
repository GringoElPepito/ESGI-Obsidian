Le modèle OSI est une représentation théorique du fonctionnement des communications réseaux d'un système d'information.
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
- [[Couche 3 - Réseau]] :
	- PDU : Paquet / Packet
	- Data Delivery  -> routed/routable protocol -> mécanisme d'adressage hiérarchique.  IPv4 / IPv6 / IPX / AppleTalk
	- Path Selection -> routing protocol Static / RIP / OSPF / EIGRP / IS-IS / BGP
	- Matériel : Router, MLS (Multi Layer Switch)
- [[Couche 2 - Liaison de données]] : 
	- PDU : Trames / Frame
	- Linear / Flat Addressing
	- Hop by Hop delivery. 
	- S'adapte à l'architecture utilisée sur un segment réseau (ex: passer de Ethernet à xDSL)
	- LAN : Ethernet, Token Ring, FDDI
	- WAN : PPP, RNIS, xDSL, HDLC
	- Matériel : Bridge, Switch
- [[Couche 1 - Physique]] :
	- PDU : Bits
	- Codage / Décodage du signal électrique, lumineux ou hertzien.
	- Matériel : Media, Repeater, Transceiver, Hub
Les couches 7 à 5 sont plutôt destinés aux développeurs.
Les couches 4 à 1 concernent quant à elles le réseau en lui même

La descente du modèle OSI (envoie de données) correspond à l'encapsulation.
La monté du modèle OSI (réception de données) correspond à la désencapsulation.
(cf [[Encapsulation-Desencapsulation]])