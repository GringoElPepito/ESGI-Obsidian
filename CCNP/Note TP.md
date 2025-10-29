# Sommaire

# Configuration de base
Passer en mode exécution :
```config
enable
```

Passer en mode configuration :
```

```

# Interface


# VLAN
Création du vlan 10 et Entrée dans la configuration du vlan 10 :
```config
vlan 10
```

Nommage du VLAN
```config
name Management
```



# EIGRP
Rentrer dans la configuration de EIGRP pour l'AS 1 :
```conf
router eigrp 1
```

Rentrer dans la configuration de EIGRP IPv6 :
```config
ipv6 router eigrp 1
```

Spécifier le router-id : 
```config
eigrp router-id 2.2.2.2
```

Activer le résumer des routes réseaux, **A réaliser sur une interface connecté à un autre routeur EIGRP** :
```config
ip summary-address eigrp 1 172.31.8.0 255.255.252.0
```