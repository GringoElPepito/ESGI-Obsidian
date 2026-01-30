# R0
```R0
en
conf t
hostname R0
no ip domain-lookup
ip domain-name cisco.lan
ip routing
ipv6 unicast-routing
key chain ospf-key
key 1
cryptographic-algorithm hmac-sha-512
key-string c1$c0-c1$c0
aaa new-model
int Gi0/0
ip add 192.168.1.254 255.255.255.0
ipv6 enable
ipv6 add 2001:DB8:1:1::FE/64
ipv6 ospf 101 area 1
no shutdown
int Gi0/1
ip add 172.16.0.1 255.255.0.0
ip ospf authentication key-chain ospf-key
ipv6 add 2001:DB8:16:16::1/64
ipv6 ospf 101 area 0
ipv6 rip 101 enable
no shutdown
int Gi0/2
ip add 172.19.0.1 255.255.0.0
ip ospf authentication key-chain ospf-key
ipv6 add 2001:DB8:19:19::1/64
ipv6 ospf 101 area 0
ipv6 rip 101 enable
no shutdown
exit
router eigrp 100
passive-interface Gi0/0
network 192.168.1.0
network 172.16.0.0
network 172.19.0.0
exit
router ospf 100
router-id 1.1.1.1
area 0 authentication
passive-interface Gi0/0
network 192.168.1.0 0.0.0.255 area 1
network 172.16.0.0 0.0.255.255 area 0
network 172.19.0.0 0.0.255.255 area 0
exit
router rip
version 2
passive-interface Gi0/0
network 192.168.1.0
network 172.16.0.0
network 172.19.0.0
exit
ipv6 router eigrp 101
passive-interface gi0/0
ipv6 router ospf 101
router-id 1.1.1.1
passive-interface gi0/0
ipv6 router rip 101
exit
do wr
```

# R1
```R1
en
conf t
hostname R1
no ip domain-lookup
ip domain-name cisco.lan
ip routing
ipv6 unicast-routing
key chain ospf-key
key 1
cryptographic-algorithm hmac-sha-512
key-string c1$c0-c1$c0
aaa new-model
int Gi0/0
ip add 192.168.2.254 255.255.255.0
ipv6 enable
ipv6 add 2001:DB8:2:2::FE/64
ipv6 ospf 101 area 2
no shutdown
int Gi0/1
ip add 172.16.0.2 255.255.0.0
ip ospf authentication key-chain ospf-key
ipv6 add 2001:DB8:16:16::2/64
ipv6 ospf 101 area 0
ipv6 rip 101 enable
no shutdown
int Gi0/2
ip add 172.17.0.1 255.255.0.0
ip ospf authentication key-chain ospf-key
ipv6 add 2001:DB8:17:17::1/64
ipv6 ospf 101 area 0
ipv6 rip 101 enable
no shutdown
exit
router eigrp 100
passive-interface Gi0/0
network 192.168.2.0
network 172.16.0.0
network 172.17.0.0
exit
router ospf 100
router-id 2.2.2.2
area 0 authentication
passive-interface Gi0/0
network 192.168.2.0 0.0.0.255 area 2
network 172.16.0.0 0.0.255.255 area 0
network 172.17.0.0 0.0.255.255 area 0
exit
router rip
version 2
passive-interface Gi0/0
network 192.168.2.0
network 172.16.0.0
network 172.17.0.0
exit
ipv6 router eigrp 101
passive-interface gi0/0
ipv6 router ospf 101
router-id 2.2.2.2
passive-interface gi0/0
ipv6 router rip 101
exit
do wr
```