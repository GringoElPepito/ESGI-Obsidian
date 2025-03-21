## HRSP
Proprio Cisco permet à plusieurs passerelles d'apparaître comme une seule grâce à une VIP (IP virtuel).

Un router est Active : celui qui à la prio HSRP la plus élevée ou (si même prio) l'IO réelle la plus élevé.
Prio par défaut est de 100 valeur compris entre 1 et 255, 0 empêche le routeur de fonctionner en HSRP
Par défaut la préemption n'est pas activée sur HSRP -> GW1 tombe. GW2 devient le nouvel active. Quand GW1 remonte, elle ne redevient pas l'Active

messages Hello envoyés toutes les 3s et considéré comme dead à partir de 10s

Lorsque que le routeur en standby considère que l'active est dead, celui-ci envoie des `bogus frame` (fausse trame), pour indiquer au commutateur que la MAC virtuelle de l'ip du HSRP a changé de port.

Traquer un lien :
```cisco
track 10 int s1/0 ip routing
int e0/0
standby 1 track 10 decrement 101
```

