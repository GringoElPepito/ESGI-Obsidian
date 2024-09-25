afficher les routes 
```cisco
show ip route
[OUTPUT Example]
S*    0.0.0.0/0 [1/0] via 1.1.1.2
      1.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        1.1.1.0/30 is directly connected, Serial1/0
L        1.1.1.1/32 is directly connected, Serial1/0
      192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.10.0/24 is directly connected, Ethernet0/0
L        192.168.10.254/32 is directly connected, Ethernet0/0
```

Ajouter une route
```cisco
conf t
ip route a.b.c.d w.x.y.z { gatewayIP or int de sortie }
ex : ip route 1.1.1.4 255.255.255.252 1.1.1.2
```