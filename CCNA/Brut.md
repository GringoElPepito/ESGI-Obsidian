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
ip access
```