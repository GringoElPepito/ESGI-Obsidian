# R0
```R0
en
conf t
hostname R0
no ip domain-lookup
ip domain-name cisco.lan
ip routing
ipv6 unicast-routing
aaa new-model
int Gi0/0
ip add 192.168.1.254 255.255.255.0
ipv6 add 2001:DB8:1:1::FE
```