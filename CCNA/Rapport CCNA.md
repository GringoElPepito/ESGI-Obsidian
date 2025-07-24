# Rapport CCNA

## Configuration

### SW1
```conf
!
! Last configuration change at 21:21:45 UTC Thu Jul 24 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW1
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
no ip routing
!
!
!
no ip cef
no ipv6 cef
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
spanning-tree vlan 1-6 priority 4096
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
interface GigabitEthernet0/0
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/2
 no shutdown
 switchport access vlan 2
 switchport mode access
 negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet0/3
 no shutdown
 switchport access vlan 6
 switchport mode access
 negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet1/0
 no shutdown
 negotiation auto
!
interface GigabitEthernet1/1
 no shutdown
 negotiation auto
!
interface GigabitEthernet1/2
 no shutdown
 negotiation auto
!
interface GigabitEthernet1/3
 no shutdown
 negotiation auto
!
interface Vlan1
 no shutdown
 ip address 192.168.1.1 255.255.255.0
 no ip route-cache
!
ip default-gateway 192.168.1.254
ip forward-protocol nd
!
ip http server
ip http secure-server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
!
control-plane
!
!
line con 0
line aux 0
line vty 0 4
 login
!
```

### SW2
```conf
!
! Last configuration change at 21:21:42 UTC Thu Jul 24 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW2
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
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
!
!
spanning-tree mode pvst
spanning-tree extend system-id
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
interface Port-channel1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface GigabitEthernet0/0
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
 channel-protocol lacp
 channel-group 1 mode active
!
interface GigabitEthernet0/1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
 channel-protocol lacp
 channel-group 1 mode active
!
interface GigabitEthernet0/2
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/3
 no shutdown
 switchport access vlan 4
 switchport mode access
 negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet1/0
 no shutdown
 negotiation auto
!
interface GigabitEthernet1/1
 no shutdown
 negotiation auto
!
interface GigabitEthernet1/2
 no shutdown
 negotiation auto
!
interface GigabitEthernet1/3
 no shutdown
 negotiation auto
!
interface Vlan1
 no shutdown
 ip address 192.168.1.2 255.255.255.0
!
ip default-gateway 192.168.1.254
ip forward-protocol nd
!
ip http server
ip http secure-server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
!
control-plane
!
!
line con 0
line aux 0
line vty 0 4
 login
!

```

### SW3
```conf
!
! Last configuration change at 21:21:39 UTC Thu Jul 24 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW3
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
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
!
!
spanning-tree mode pvst
spanning-tree extend system-id
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
interface Port-channel1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface GigabitEthernet0/0
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
 channel-protocol lacp
 channel-group 1 mode active
!
interface GigabitEthernet0/1
 no shutdown
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
 channel-protocol lacp
 channel-group 1 mode active
!
interface GigabitEthernet0/2
 no shutdown
 switchport access vlan 5
 switchport mode access
 switchport port-security violation protect
 switchport port-security mac-address 0050.7966.680f
 negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet0/3
 no shutdown
 switchport access vlan 3
 switchport mode access
 negotiation auto
 spanning-tree portfast edge
!
interface GigabitEthernet1/0
 no shutdown
 negotiation auto
!
interface GigabitEthernet1/1
 no shutdown
 negotiation auto
!
interface GigabitEthernet1/2
 no shutdown
 negotiation auto
!
interface GigabitEthernet1/3
 no shutdown
 negotiation auto
!
interface Vlan1
 no shutdown
 ip address 192.168.1.3 255.255.255.0
!
ip default-gateway 192.168.1.254
ip forward-protocol nd
!
ip http server
ip http secure-server
!
ip ssh server algorithm encryption aes128-ctr aes192-ctr aes256-ctr
ip ssh client algorithm encryption aes128-ctr aes192-ctr aes256-ctr
!
!
!
!
!
!
control-plane
!
!
line con 0
line aux 0
line vty 0 4
 login
!
```

### DEFENSE
```conf
!
version 15.9
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname DEFENSE
!
boot-start-marker
boot-end-marker
!
!
enable secret 9 $9$hc4UpltGAmZ72z$V7AADqp3tb4P6.feys1RgvuBk8vLraqmpGE.Ikz4pHk
!
no aaa new-model
!
!
!
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
ip dhcp pool Administrateurs
 network 192.168.2.0 255.255.255.0
 dns-server 192.168.13.250 
 default-router 192.168.2.254 
!
ip dhcp pool VoIP
 network 192.168.3.0 255.255.255.0
 default-router 192.168.3.254 
 dns-server 192.168.13.250 
!
ip dhcp pool Comptables
 network 192.168.4.0 255.255.255.0
 default-router 192.168.4.254 
 dns-server 192.168.13.250 
!
ip dhcp pool Paye
 network 192.168.5.0 255.255.255.0
 default-router 192.168.5.254 
 dns-server 192.168.13.250 
!
ip dhcp pool Imprimantes
 network 192.168.6.0 255.255.255.0
 default-router 192.168.6.254 
 dns-server 192.168.13.250 
!
!
!
ip host PC12.64 192.168.12.64
ip name-server 192.168.13.250
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
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
interface GigabitEthernet0/0
 no shutdown
 ip address 192.168.1.254 255.255.255.0
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/0.2
 no shutdown
 encapsulation dot1Q 2
 ip address 192.168.2.254 255.255.255.0
!
interface GigabitEthernet0/0.3
 no shutdown
 encapsulation dot1Q 3
 ip address 192.168.3.254 255.255.255.0
!
interface GigabitEthernet0/0.4
 no shutdown
 encapsulation dot1Q 4
 ip address 192.168.4.254 255.255.255.0
!
interface GigabitEthernet0/0.5
 no shutdown
 encapsulation dot1Q 5
 ip address 192.168.5.254 255.255.255.0
!
interface GigabitEthernet0/0.6
 no shutdown
 encapsulation dot1Q 6
 ip address 192.168.6.254 255.255.255.0
!
interface GigabitEthernet0/1
 no shutdown
 ip address 2.2.2.1 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/2
 no shutdown
 ip address 1.1.1.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/3
 no shutdown
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
router ospf 1
 passive-interface GigabitEthernet0/0
 network 1.1.1.0 0.0.0.3 area 0
 network 2.2.2.0 0.0.0.3 area 0
 network 192.168.1.0 0.0.0.255 area 0
 network 192.168.2.0 0.0.0.255 area 0
 network 192.168.3.0 0.0.0.255 area 0
 network 192.168.4.0 0.0.0.255 area 0
 network 192.168.5.0 0.0.0.255 area 0
 network 192.168.6.0 0.0.0.255 area 0
 default-information originate always
!
ip forward-protocol nd
!
!
no ip http server
ip nat inside source list ACLPAT interface GigabitEthernet0/1 overload
ip route 0.0.0.0 0.0.0.0 2.2.2.2
!
ip access-list standard ACLPAT
 permit 192.168.12.0 0.0.0.255
!
ipv6 ioam timestamp
!
!
!
control-plane
!
banner exec ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
banner incoming ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
banner login ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
banner motd ^C
Bienvenue sur le Routeur DEFENSE
Usage reserve au domaine ccna.lan
^C
!
line con 0
 password cisco123
line aux 0
line vty 0 4
 password cisco123
 login
 transport input none
line vty 5 15
 password cisco123
 login
 transport input none
!
no scheduler allocate
!
end
```

### FAI
```conf
!
version 15.9
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname FAI
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
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
!
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
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
interface GigabitEthernet0/0
 no shutdown
 ip address 2.2.2.2 255.255.255.252
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/1
 no shutdown
 ip address 5.5.5.254 255.255.255.0
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/2
 no shutdown
 ip address dhcp
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/3
 no shutdown
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
ip forward-protocol nd
!
!
no ip http server
!
ipv6 ioam timestamp
!
!
!
control-plane
!
!
line con 0
line aux 0
line vty 0 4
 login
 transport input none
!
no scheduler allocate
!
end
```

### Paris 
```conf
!
version 15.9
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname PARIS
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
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
!
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
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
interface GigabitEthernet0/0
 no shutdown
 ip address 1.1.1.2 255.255.255.252
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/1
 no shutdown
 ip address 10.1.1.1 255.255.255.248
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/2
 no shutdown
 ip address 192.168.11.254 255.255.255.0
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/3
 no shutdown
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
router ospf 1
 passive-interface GigabitEthernet0/2
 network 1.1.1.0 0.0.0.3 area 0
 network 10.1.1.0 0.0.0.7 area 0
 network 192.168.11.0 0.0.0.255 area 0
!
ip forward-protocol nd
!
!
no ip http server
!
ipv6 ioam timestamp
!
!
!
control-plane
!
!
line con 0
line aux 0
line vty 0 4
 login
 transport input none
!
no scheduler allocate
!
end
```

### Lyon
```conf
!
version 15.9
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname LYON
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
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
!
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
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
interface GigabitEthernet0/0
 no shutdown
 ip address 10.1.1.3 255.255.255.248
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/1
 no shutdown
 ip address 192.168.13.254 255.255.255.0
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/2
 no shutdown
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/3
 no shutdown
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
router ospf 1
 passive-interface GigabitEthernet0/1
 network 10.1.1.0 0.0.0.7 area 0
 network 192.168.13.0 0.0.0.255 area 0
!
ip forward-protocol nd
!
!
no ip http server
!
ipv6 ioam timestamp
!
!
!
control-plane
!
!
line con 0
line aux 0
line vty 0 4
 login
 transport input none
!
no scheduler allocate
!
end
```

### Reims
```conf
!
! Last configuration change at 21:26:13 UTC Thu Jul 24 2025
!
version 15.9
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname REIMS
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
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
!
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
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
interface GigabitEthernet0/0
 no shutdown
 ip address 10.1.1.2 255.255.255.248
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/1
 no shutdown
 ip address 192.168.12.254 255.255.255.0
 ip access-group LAN_Reims in
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/2
 no shutdown
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/3
 no shutdown
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
router ospf 1
 passive-interface GigabitEthernet0/1
 network 10.1.1.0 0.0.0.7 area 0
 network 192.168.12.0 0.0.0.255 area 0
!
ip forward-protocol nd
!
!
no ip http server
!
ip access-list extended LAN_Reims
 permit ip host 192.168.12.1 192.168.13.0 0.0.0.255 log
 permit tcp 192.168.12.64 0.0.0.7 host 192.168.13.250 eq www log
 permit tcp 192.168.12.64 0.0.0.7 host 192.168.13.250 eq 443 log
 permit udp 192.168.12.64 0.0.0.7 host 192.168.13.250 eq domain log
 deny   ip 192.168.12.0 0.0.0.255 192.168.13.0 0.0.0.255 log
 permit ip 192.168.12.0 0.0.0.255 any log
!
ipv6 ioam timestamp
!
!
!
control-plane
!
!
line con 0
line aux 0
line vty 0 4
 login
 transport input none
!
no scheduler allocate
!
end
```

### DNS
```conf
!
version 15.9
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname DNS
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
!
!
!
!
!
no ip routing
!
!
!
!
!
!
ip host www.google.fr 5.5.5.1
no ip cef
no ipv6 cef
!
multilink bundle-name authenticated
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
interface GigabitEthernet0/0
 no shutdown
 ip address 192.168.13.250 255.255.255.0
 no ip route-cache
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/1
 no shutdown
 no ip address
 no ip route-cache
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/2
 no shutdown
 no ip address
 no ip route-cache
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/3
 no shutdown
 no ip address
 no ip route-cache
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
ip default-gateway 192.168.13.254
ip forward-protocol nd
!
!
no ip http server
ip dns server
!
ipv6 ioam timestamp
!
!
!
control-plane
!
!
line con 0
line aux 0
line vty 0 4
 login
 transport input none
!
no scheduler allocate
!
end
```

