# Introduction

# Finance

## Virtualisation
6 serveurs de f
Serveur PowerEdge R6615 Serveur Rack Plus :
- AMD EPYC 9354P 3,25 GHz 32C/64T x 1
- 32 Go RDIMM, 5600 MT/s x 2
- SSD 960Go DATA x 2

## Stockage :

# Méthodologie

# Technique

## Stockage
RAIDZ2 type de RAID fonctionnement uniquement avec le système de fichier ZFS
## Réseau
### LAN
Le routage est assuré par 2 routeurs cisco qui font le liens entre les pare-feux interne et le LAN.
2 NETCORE se charge de la centralisation du trafic réseau du LAN en servant d'intermédiaire entre les routeurs et les switch de distribution
Les switch de distribution sont connectés aux 2 NETCORE et fournissent l'accès au réseau aux terminaux (poste utilisateur, imprimantes etc...)

### WAN
2 firewalls externes OPNsense
2 firewalls internes Pfsense

### DMZ
