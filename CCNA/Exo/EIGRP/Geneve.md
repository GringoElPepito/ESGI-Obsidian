```
!
! Last configuration change at 10:34:05 UTC Thu Sep 26 2024
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname Geneve
!
boot-start-marker
boot-end-marker
!
aqm-register-fnf
!
!
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
!
!
!
!
!
!


!
!
!
!
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
redundancy
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 no shutdown
 ip address 192.168.86.254 255.255.255.0
 no keepalive
!
interface Ethernet0/1
 no shutdown
 ip address 192.168.87.254 255.255.255.0
 no keepalive
!
interface Ethernet0/2
 no shutdown
 no ip address
 shutdown
!
interface Ethernet0/3
 no shutdown
 no ip address
 shutdown
!
interface Serial1/0
 no shutdown
 ip address 1.1.1.10 255.255.255.252
 serial restart-delay 0
!
interface Serial1/1
 no shutdown
 ip address 1.1.1.13 255.255.255.252
 serial restart-delay 0
!
interface Serial1/2
 no shutdown
 no ip address
 shutdown
 serial restart-delay 0
!
interface Serial1/3
 no shutdown
 no ip address
 shutdown
 serial restart-delay 0
!
!
router eigrp 100
 network 1.1.1.8 0.0.0.3
 network 1.1.1.12 0.0.0.3
 network 192.168.86.0
 network 192.168.87.0
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
!
!
!
!
control-plane
!
!
!
!
!
!
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
!
!
end
```