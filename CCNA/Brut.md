Une interface de loopback est une interface virtuelle que l'on utilise pour administrer un routeur, permet ainsi de joindre le matériel sans se soucier de l'état des interfaces réelles.
Il est possible de créer une interface de loopback
```cisco
int loop 0
ip add 172.16.1.1/32
```
L'interface est annoncé dans mon routage

## DMZ
LAN = Réseau de confiance forte, car on peut y mettre du AAA (Authentication, Authorization, Accounting).
DMZ = Réseau de confiance moyenne, réseau dans lequel les hôtes peuvent uniquement répondre à des paquets et non pas envoyer des paquets à leur propre initiative (fonctionnement théorique). Autant de DMZ que de politique d'accès
WAN = Réseau de confiance nulle

## NAT
RFC1918, défini les IP Privées :
- 10.x.y.z/8
- 172.16.y.z/12 (172.16 à 172.31)
- 192.168.y.z/16
3 grands types de NAT :
- NAT : One-to-One mapping -> une IP publique pour une IP privée, Ne change que l'IP, IP de destination ou IP source
(FW)=\=\=\=\=\=(CPE)=\=\=\=\=\={ISP}
CPE = Customer Premises Equipement ou CE = Customer Edge
Le One-to-One mapping sert principalement à rendre le CPE invisible, car si on enlève le DPE l'opérateur ne garantit plus la GTR (garantie de temps de rétablissement).
- PAT Dynamique (source NAT) : Modifie l'IP source et le port source (par un port dédié à une session sortant vers le WAN) pour permettre à X IP privées d'être translatées sur une ou un pool d'IP publique (Interface WAN soit bloc IP publiques routées vers le client).
- PAT statique (destination NAT) Modifier l'IP de destination généralement publique pour un port particulier -> hébergement

### Configuration PAT Dynamique
- Configurer ACL pour définir le ou les réseaux privées qui pourront être translatés
- Configurer les interfaces pour le NAT (l'operation de NAT ne peut avoir lieu qu'entre une interface inside et une interface outside)
- Créer la règle de NAT
```cisco
en
conf t
ip access-list standard ACLNAT
permit 192.168.10.0 0.0.0.255
exit
int e0/0
ip nat inside
int s1/0
ip nat outside
exit
ip nat inside source list ACLNAT int s1/0 overload
do wr
```
Pour voir les traductions NAT en cours :
```cisco
show ip nat translation
```
`CTRL` + `SHIFT` + `9 (pas celui du pavé numérique)` pour quitter le domain name translating
Configuration du PAT statique :
- Configurer les interfaces pour le NAT
- Configurer la redirection de port
```
en
conf t
int e0/1
ip nat inside
exit
ip nat inside source static tcp 192.168.11.200 23 5.5.5.7 23
do wr
```

## ACL
Sécurité de base sur un réseau.
Les ACL sont stateless -> pas de table d'état. Chaque paquet est traité individuellement. Un flux interdit dans un sens l'est aussi dans l'autre.

3 types d'ACL
- Standard : numéro compris entre 1 & 99. On peut seulement vérifier l'IP source
- Etendue : numéro compris entre 1 & 99. Vérifie IPs, IPd, Numéro de protocole IP, Port source, Port destination
- Nommée : qui est soit standard soit étendue
Dans les ACL, aux IP, on fait correspondre un Wildcard mask
ACL : collection séquentielle (l'ordre à une importance) d'instructions vérifiant des paramètres aboutissant à une autorisation ou un refus. (DENY ANY implicite à la fin de chaque ACL). Il faut aller du plus précis au plus général

Sur un router, on peut appliquer UNE SEULE ACL par sens (in ou out), par interface par protocole routé (IPv4,IPv6).

Configuration ACL Standard :
```ACL
(config)# access-list X { permit | deny } IP_SOURCE WILDCARD_SOURCE
```
Configuration ACL Etendu :
```ACL
(config)# access-list X { permit | deny } IP_SOURCE WILDCARD_SOURCE IP_DESTINATION WILDCARD_DESTINATION [ operator operand ]
```
- Opérateur = eq, neq, gt, lt, range
- Opérance = numéro de port, type ICMP
Configuration ACL nommée :
```ACL
(config)# ip access-list { standard | extended } NOM
(config-nacl)# { deny | permit }
```
Appliquer l'ACL à une interface :
```cisco
(config-if)# ip access-group { N°|Nom } {in|out}
```
Vérifier une IP unicast (Exemple vérifier IP 10.1.1.1) :
Dans l'ACL : 10.1.1.1 0.0.0.0 -> peut être remplacé par host 10.1.1.1

Vérifier toutes les IP :
Dans l'ACL : 0.0.0.0 255.255.255.255 -> peut être remplacé par `any`


### EXO ACL Correction

```cisco
ip access-list extended ACLCORPO
permit ip host 192.168.10.1 192.168.11.0 0.0.0.255
permit tcp 192.168.10.0 0.0.0.255 host 192.168.11.200 eq telnet
deny ip 192.168.10.0 0.0.0.255 192.168.11.0 0.0.0.255 log
int e0/0
ip access-group ACLCORPO in
exit

ip access-list extended ACLDMZ
deny ip 192.168.11.0 0.0.0.255 any log
int e0/1
ip access-group ACLDMZ in
exit

ip access-list extended ACLWAN
permit tcp any host 5.5.5.7 eq telnet
deny ip any any log
int s1/0
ip access-group ACLWAN
```
`telnet` équivaut au port 23

Création de règles d'inspections :
```cisco
ip inspect name INSP_CORPO tcp
ip inspect name INSP_CORPO udp
ip inspect name INSP_CORPO icmp
int e0/0
ip inspect INSP_CORPO in
exit
ip inspect name INSP_WAN telnet
int s1/0
ip inspect INSP_WAN in
exit
do wr
```

Router On A Stick = router faisant du routage inter vlan
1 VLAN = 1 réseau IP/ domaine de broadcast
Les Trunks : lien physique transportant plusieurs VLANs -> il est nécessaire d'étiqueter/taguer les trames pour identifier leur VLAN d'appartenance. 
Norme 802.1q ou dot1q
Chez Cisco tous les VLANs connu du switch (présent dans la VLAN Database) sont autorisés par défaut sur les trunks
Pour traiter les VLANs (Lire et /ou écrire le tag), le switch doit connaître ces VLANs. donc typiquement dans un LAN tous les switchs ont la même base de données des VLANs.
Chez Cisco les VLANs dans le fichier de configuration, ils sont stockés dans flash:vlan.dat.
Par défaut, tous les ports d'un switch sont dans le VLAN 1 -> VLAN Natif (untagged).

Partage de la base de données des VLANs : VTP => Virtual Trunking Protocol
VTP est un protocole propriétaire Cisco.
Il existe 3 modes :
- Serveur : créer, modifier, supprimer, apprendre les VLANs et relayer les messages VTP. Mode par défaut
- Client : apprendre les VLANs et relayer les messages VTP.
- transparent : créer, modifier, supprimer des VLANs mais ces derniers restent locaux à ce switch et il relaye les messages VTP.
VTP fonctionne avec un numéro de révision ; à l'origine = 1
A chaque modification de VLAN, le switch envoie un message VTP et le numéro avec le numéro de révision incrémenté de 1.
Les messages VTP ne s'échangent que sur les trunks
Pour configurer VTP :
Les switchs doivent avoir le même nom de domaine VTP, le même mot de passe et on peut préciser le mode.
```cisco
en
conf t
vtp domain ESGI
vtp password toto
```
Pour réinitialiser le numéro de révision :
- passer le sw en mode transparent puis le repasser dans le mode voulu
- ou changer le nom de domaine VTP puis remettre le bon

### Trunks
Protocole proprio Cisco : DTP = Dynamic Trunking Protocol
Négocier automatique un trunk entre 2 switchs
```cisco
(config-if)#switchport encapsulation dot1q
(config-if)#switchport mode { dynamic desirable | dynamic auto | trunk }
```
Toutes les combinaisons aboutissent à un trunk sauf auto avec auto
Autorisé des VLANs sur un port trunk :
```cisco
(config-if)#switchport trunk allowed vlan { all | except | add | remove } VLANs
```
Configurer un port en mode access :
```cisco
(config-if)#switchport mode access vlan N°
```

## Agrégation de liens
Entre 2 switchs -> agréger 2,4,8 ou 16 liens pour augmenter la BW
2 protocoles :
- PagP : Port Aggreg Proto : proprio Cisco
- LACP : Link Aggre Control Protocol 802.3ad
Algo de distribution entre les 2 switchs pour utiliser les x liens, il y a 2 familles d'algo :
- Binaire : basé sur un seul paramètre (IP-source, IP-destination, Mac-source, Mac-destination, Port-source, Port-destination).
- Hashage : XOR entre 2 paramètres (IP-source XOR IP-destination, Mac-source XOR Mac-destination, Port-source XOR Port-destination).
A noter : les 2 switchs ne doivent pas forcément utiliser le même algo
- Les ports d'un bundle doivent avoir la même vitesse.
- Si c'est un trunk, il faut forcer en mode trunk (pas de dynamic possible)
- Avoir les même VLANs autorisés
Configurer un port channel :
Choix de l'algo
```cisco
port-channel load-balance [algo]
```
Bundlisation des ports :
```cisco
int range e1/2-3
channel-protocol { pagp | lacp(lacp) }
channel-group 1 mode { desirable(pour pagp) | auto(pour pagp) | active(lacp) | passive(lacp) }
```
Visualtion du port-channel :
```cisco
sh etherchannel summ
```
Visualisation load-balancing :
```cisco
sh ethernetchannel load-balance
```

## Spanning-tree
STP = Spanning-Tree Protocol 

Explication transmission paquet :
1. Quand un switch reçoit une trame, celui-ci renseigne l'adresse MAC source dans la table CAM en l'associant avec le port d'où vient la trame.

Création de l'arbre STP : 

## EVE-NG
Pour mettre internet dans une maquette EVE-NG 



