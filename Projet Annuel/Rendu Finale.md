# Introduction

# Finance

# Méthodologie

# Technique
## Réseau
### LAN
Le routage est assuré par 2 routeurs cisco qui font le liens entre les pare-feux interne et le LAN.
2 NETCORE se charge de la centralisation du trafic réseau du LAN en servant d'intermédiaire entre les routeurs et les switch de distribution
Les switch de distribution sont connectés aux 2 NETCORE et fournissent l'accès au réseau aux terminaux (poste utilisateur, imprimantes etc...)

### WAN
2 firewalls externes OPNsense
2 firewalls internes Pfsense

### DMZ
