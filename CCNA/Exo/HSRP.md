# ISP
```cisco
en
conf t
host ISP
int s1/0
ip add 1.1.1.2 255.255.255.0
no sh
int s1/1
ip add 2.2.2.2 255.255.255.0
no sh
int loop 0
ip add 8.8.8.8
```