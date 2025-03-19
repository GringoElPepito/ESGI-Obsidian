# SW1
```cisco
spanning-tree vlan 1 priority 4096
int e0/3
spanning-tree vlan 1 port-priority 64
do wr
```

# SW2
```cisco
spanning-tree vlan 2 priority 4096
int e0/1
spanning-tree priority 64
```

# SW3
```cisco
spanning-tree vlan 1 priority 8192
int e0/1
spanning-tree vlan 1 port-priority 64
do wr
spanning-tree vlan 3 priority 4096
spanning-tree vlan 4 priority 8192
```