# Réseau docker
les types de réseaux :
- Bridge - équivalent NAT, réseau par défaut sur docker
	- Le réseau créé par défaut est celui-ci 172.17.0.0/24, puis incrémentation pour les autres réseaux bridge (172.18.0.0/24, 172.19.0.0/24 etc...), il est possible de choisir son propre adressage grâce à l'option `--subnet` , ip par défaut de l'hôte première adresse dispo (172.17.0.1), l'adresse de l'hôte sert de DNS interne à Docker et de gateway.
- host - Récupère l'IP de l'hôte utile pour de la supervision, DHCP, DNS, élément nécessitant beaucoup de configuration réseau ou du broadcast. 
- none (NULL)
- overlay
- user-defined bridge
- macvlan (bridge)
- macvlan (802.1q)
- IP VLAN (L2/L3)

Mappage de port

Lister les types de réseaux utilisés par les conteneurs actifs
```bash
docker network ls
```

Assignation d'un conteneur
```bash
docker run --rm -ti --network
```

```bash
netstat -paunt
```
A noter que les process docker ont un PID à 6 chiffres ou plus


