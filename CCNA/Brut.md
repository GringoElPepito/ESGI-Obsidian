Une interface de loopback est une interface virtuelle que l'on utilise pour administrer un routeur, permet ainsi de joindre le matériel sans se soucier de l'état des interfaces réelles.
Il est possible de créer une interface de loopback
```cisco
int loop 0
ip add 172.16.1.1/32
```
L'interface est annoncé dans mon routage