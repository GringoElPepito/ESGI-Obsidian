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
