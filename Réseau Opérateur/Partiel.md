# Loopback
- R1 : 1.1.1.1/32
- R2 : 2.2.2.2/32
- R3 : 3.3.3.3/32
- R4 : 4.4.4.4/32
- R5 : 5.5.5.5/32
- R6 : 6.6.6.6/32
# LAN
- R1 : 192.168.10.1/24
- R2 : 192.168.20.1/24
- R3 : 192.168.30.1/24
- R4 : 192.168.40.1/24
- R5 : 192.168.50.1/24
- R6 : 192.168.60.1/24

# WAN
- R1 - R2 : 10.0.0.0/30
- R1 - R3 : 10.0.0.4/30
- R2 - R3 : 10.0.0.8/30
- R2 - R4 : 10.0.0.12/30
- R4 - R5 : 10.0.0.16/30
- R4 - R6 : 10.0.0.20/30
# R1
```conf
hostname R1
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
interface Loopback0
 ip address 1.1.1.1 255.255.255.255
!
interface Ethernet0/0
 ip address 10.0.0.1 255.255.255.252
!
interface Ethernet0/1
 ip address 10.0.0.5 255.255.255.252
!
interface Ethernet0/2
 ip address 192.168.10.1 255.255.255.0
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
 passive-interface Ethernet0/2
 network 1.1.1.1 0.0.0.0 area 0
 network 10.0.0.0 0.0.0.3 area 0
 network 10.0.0.4 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
!
router bgp 65001
 bgp log-neighbor-changes
 network 10.0.0.0 mask 255.255.255.252
 network 10.0.0.4 mask 255.255.255.252
 network 192.168.10.0
 neighbor 2.2.2.2 remote-as 65001
 neighbor 2.2.2.2 update-source Loopback0
 neighbor 3.3.3.3 remote-as 65001
 neighbor 3.3.3.3 update-source Loopback0
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

# R2
```conf

```

# R3
```conf

```

# R4
```conf

```

# R5
```conf

```

# R6
```conf

```