Configurer les 2 sites. PAT dynamique sur leur interface WAN + DHCP + route par défaut vers ISP. 


SITE 1:
```
en
conf t
hostname SITE1
no ip domain-lookup
int e0/0
ip add 192.168.10.254 255.255.255.0
no shut
int s1/0
ip add 1.1.1.1 255.255.255.252
no shut

```