# Sommaire

# Configuration de base
Passer en mode exécution :
```config
enable
```

Passer en mode configuration :
```config
configure terminal
```

Changer le nom d'hôte de l'équipement :
```config
hostname SW1
```

Désactiver la résolution de nom de domaine :
```config
no ip domain-lookup
```

# VLAN
Création du vlan 10 et Entrée dans la configuration du vlan 10 :
```config
vlan 10
```

Nommage du VLAN
```config
name Management
```

# Interface

Activation de l'interface :
```config
no shutdown
```

Attribution d'une adresse IP sur une interface :
Pour les routeurs -> uniquement faisable sur des interfaces ou sous-interfaces physiques
Pour les switches -> uniquement faisable sur des interfaces vlan
```config
ip address 172.16.0.1 255.255.255.0
```
## Interface de Port physique
Entrer dans la configuration d'un port :
```config
interface fastEthernet0/1
```

Entrer dans la configuration de plusieurs ports avec des numéros se suivants :
```config
interface fastEthernet0/1-24
```

Entrer dans la configuration de plusieurs ports avec des numéros ne se suivants pas :
```config
interface fastEthernet0/1,fastEthernet0/10,fastEthernet0/20
```

Passage du ou des ports en mode `access` :
```config
switchport mode access
```

Assignation du vlan 10 sur un port en `access` :
```config
switchport access vlan 10
```

Passage du ou des ports en mode `trunk` :
```config
switchport mode trunk
```

Activer l'encapsulation dot1q **uniquement pour les switches L3** :
```config
switchport trunk encapsulation dot1q
```

Définition du vlan 10 en vlan natif sur un ou plusieurs ports en mode  `trunk` :
```config
switchport trunk native vlan 10
```

Définition de la liste des vlan autorisés sur un ou plusieurs ports en mode `trunk`
Liste des vlan autorisé par la commande suivante : 10,11,12,50
```config
switchport trunk allowed vlan 10-12,50
```

## Interface VLAN


# STP

Action du Rapid PVST+ :
```config
spanning-tree mode rapid-pvst
```

Passage swi en prioritaire secondaire :
```config
spanning-tree vlan 1,10,20,30,40,50,60,70,80,99 root secondary
```

==A réaliser sur une configuration de port **PAS CONNECTER A UN SWITCH**==
Activation du `spanning-tree` en mode `portfast` sur un port **A réaliser dans la configuration d'un ou plusieurs ports** :
```config
spanning-tree portfast
```

==A réaliser sur une configuration de port **PAS CONNECTER A UN SWITCH**==
Activation du `spanning-tree` en mode `portfast` sur un ou plusieurs ports en `trunk` :
```config
spanning-tree portfast trunk
```

==A réaliser sur une configuration de port==
Désactivation du `spanning-tree` en mode `portfast` sur un ou plusieurs ports:
```config
spanning-tree portfast disable
# ou
no spanning-tree portfast
```

==A réaliser sur une configuration de port **PAS CONNECTER A UN SWITCH**==
Activation de la BPDU Guard sur un ou plusieurs ports :
```config
spanning-tree bpduguard enable
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