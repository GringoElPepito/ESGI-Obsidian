Configurer les 2 sites. PAT dynamique sur leur interface WAN + DHCP + route par défaut vers ISP. 


SITE 1:
```
en
conf t
hostname SITE1
no ip domain-lookup
int G0/0
ip add 192.168.10.254 255.255.255.0
ip nat inside
no shut
int g0/1
ip add 1.1.1.1 255.255.255.252
ip nat outside
no shut
exit
ip access-list extended ACL_NAT
deny ip 192.168.10.0 0.0.0.255 192.168.11.0 0.0.0.255
permit ip 192.168.10.0 0.0.0.255 any
exit
ip nat inside source list ACL_NAT int g0/1 overload
ip dhcp pool LAN1
netw 192.168.10.0 255.255.255.0
default-rout 192.168.10.254
exit
ip default-g 1.1.1.2
do wr
```

SITE 2 :
```
en
conf t
hostname SITE2
no ip domain-lookup
int G0/0
ip add 192.168.11.254 255.255.255.0
ip nat inside
no shut
int g0/1
ip add 2.2.2.1 255.255.255.252
ip nat outside
no shut
exit
ip access-list extended ACL_NAT
deny ip 192.168.11.0 0.0.0.255 192.168.10.0 0.0.0.255
permit ip 192.168.11.0 0.0.0.255 any
exit
ip nat inside source list ACL_NAT int g0/1 overload
ip dhcp pool LAN2
netw 192.168.11.0 255.255.255.0
default-rout 192.168.11.254
exit
ip default-g 2.2.2.2
do wr
```

ISP : 
```
en
conf t
hostname ISP
no ip domain-lookup
int g0/1
ip add 1.1.1.2 255.255.255.252
no shut
int g0/2
ip add 2.2.2.2 255.255.255.252
no shut
int g0/0
ip add 8.8.8.254 255.255.255.0
no shut
do wr
```

PC INTERNET:
```
en
conf t
hostname PC-INTERNET
```

IPSec SITE1 :
```
!!! Etablir un tunnel sécurisé entre des LANs
!!! PHASE 1
en
conf t
crypto isakmp policy 1
authentication pre
encryption des !!! DES car LAB en PROD utiliser AES
hash md5 !!! MD5 car LAB en PROD utiliser minimum SHA-1
group 2 !!! groupe 2 car LAB en PROD minimum 14
lifetime 43200
exit
!!! Définition de la PSK
crypto isakmp key toto1234 address 2.2.2.1
!!! PHASE 2
crypto ipsec transform-set TS_VPN esp-des esp-md5
exit
!!! Définir l'interesting trafic
ip access-list exte ACLVPN
permit ip 192.168.10.0 0.0.0.255 192.168.11.0 0.0.0.255
exit
!!! Créer une cyrpto map
crypto map CM_VPN 10 ipsec-isakmp
match address ACL_VPN
set transfor TS_VPN
set peer 2.2.2.1
exit
int g0/1
crypto map CM_VPN
exit
///NAT Exemption
ip access-list extended ACL_NAT
deny ip 192.168.10.0 0.0.0.255 192.168.11.0 0.0.0.255
permit ip 192.168.10.0 0.0.0.255 any
exit
do wr
```

IPSec SITE2 :
```
!!! Etablir un tunnel sécurisé entre des LANs
!!! PHASE 1
en
conf t
crypto isakmp policy 1
authentication pre
encryption des !!! DES car LAB en PROD utiliser AES
hash md5 !!! MD5 car LAB en PROD utiliser minimum SHA-1
group 2 !!! groupe 2 car LAB en PROD minimum 14
lifetime 43200
exit
!!! Définition de la PSK
crypto isakmp key toto1234 address 1.1.1.1
!!! PHASE 2
crypto ipsec transform-set TS_VPN esp-des esp-md5
exit
!!! Définir l'interesting trafic
ip access-list exte ACL_VPN
permit ip 192.168.11.0 0.0.0.255 192.168.10.0 0.0.0.255
exit
!!! Créer une cyrpto map
crypto map CM_VPN 10 ipsec-isakmp
match address ACL_VPN
set transfor TS_VPN
set peer 1.1.1.1
exit
int e0/1
crypto map CM_VPN
exit
///NAT Exemption
ip access-list extended ACLNAT
deny ip 192.168.11.0 0.0.0.255 192.168.10.0 0.0.0.255
permit ip 192.168.11.0 0.0.0.255 any
exit
do wr
```

