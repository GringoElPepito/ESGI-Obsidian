# Info Prof
## Prof CCNA
Erwan GUILLEMOT
eguillemot@esgi.fr

## Projet annuel
kchevreuil@myges.fr
kevin.chevreuil@kasfeleia.com
kaisencas

## Netacad
Céline OULMI
Passer les examens finaux des 3 modules Cisco.


INFO Wifi :
SSID : Campus-GES
MDP : R3s3@u-G3s




1.1.1.0/30 -> Mulhouse/Brest
1.1.1.4/30 -> Mulhouse/Paris
1.1.1.8/30 -> Paris/Brest - 1
1.1.1.12/30 -> Paris/Best - 2
1.1.1.16/30 -> Brest/Orléans
1.1.1.20/30 -> Paris/Orléans

```cfg
conf t
line console 0
password 1234
login
line vty 0 4
password 1234
login
transport input ssh
exit
enable secret 1234
do wr
```