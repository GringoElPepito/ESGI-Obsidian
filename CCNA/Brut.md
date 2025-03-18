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
(config)# ip access-group { N°|Nom } {in|out}
```
Vérifier une IP unicast (Exemple vérifier IP 10.1.1.1) :
Dans l'ACL : 10.1.1.1 0.0.0.0 -> peut être remplacé par host 10.1.1.1

Vérifier toutes les IP :
Dans l'ACL : 0.0.0.0 255.255.255.255 -> peut être remplacé par `any`