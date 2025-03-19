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
int s1/0
ip add 1.1.1.1 255.255.255.252
no sh
int e0/0
ip add 192.168.10.250 255.255.255.0
no sh
do wr
```

# GW2
```cisco
en
conf t
host GW2
ip default-gateway 2.2.2.2
int s1/1
ip add 2.2.2.1 255.255.255.252
no sh

```