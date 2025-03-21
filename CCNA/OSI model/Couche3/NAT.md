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
