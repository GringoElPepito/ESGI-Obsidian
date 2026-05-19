# Réseau docker
les types de réseaux :
- Bridge - équivalent NAT
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


