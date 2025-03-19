# ISP
```cisco
en
conf t
host ISP
int s1/0
ip add 1.1.1.2 255.255.255.252
no sh
int s1/1
ip add 2.2.2.2 255.255.255.252
no sh
int loop 0
ip add 8.8.8.8 255.255.255.255
no sh
do wr
```

# GW1
```cisco
en
conf t
host GW1
ip default-gateway 1.1.1.2
ip access-list standard ACLNAT
permit 192.168.10.0 0.0.0.255
track 10 int s1/0 ip routing
int s1/0
ip add 1.1.1.1 255.255.255.252
ip nat outside
no sh
int e0/0
ip add 192.168.10.250 255.255.255.0
ip nat inside
standby 1 priority 200
standby 1 preempt
standby 1 track 10 decrement 101
no sh
exit
ip nat inside source list ACLNAT int s1/0 overload
do wr
```

# GW2
```cisco
en
conf t
host GW2
ip access-list standard ACLNAT
permit 192.168.10.0 0.0.0.255
ip default-gateway 2.2.2.2
int s1/1
ip add 2.2.2.1 255.255.255.252
ip nat outside
no sh
int e0/0
ip add 192.168.10.251 255.255.255.0
ip nat inside
standby 1 priority 
no sh
ip nat inside source list ACLNAT int s1/0 overload
do wr
```