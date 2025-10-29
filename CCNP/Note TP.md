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

Annulation d'un commande :
```config
no $commande_a_annuler
```

Changer le nom d'hôte de l'équipement :
```config
hostname SW1
```

Désactiver la résolution de nom de domaine :
```config
no ip domain-lookup
```

Sauvegarder/Enregistrer la configuration :
```config
write 
# ou
write memory
# ou 
copy runnning-config startup-config
```

Afficher la table de routage :
```config
show ip route
```

Définition de l'adresse IP 172.16.1.1 en tant que passerelle par défaut :
```config
ip default-gateway 172.16.1.1
```

Configuration d'une route statique IPv4 par défaut en passant par la passerelle 172.16.1.1 :
```config
ip route 0.0.0.0 0.0.0.0 172.16.1.1
```

Configuration d'une route statique IPv4 vers le réseau 172.16.0.0/24 via 172.16.1.1 avec une métrique à 1 :
```config
ip route 172.16.0.0 255.255.255.0 172.16.1.1 1
```

Activation du routage IPv6 :
```config
ipv6 unicast-routing
```

Configuration d'une route statique IPv6 vers le réseau 2001:DB8:1:2::/64 via 2001:DB8:1:A001::2 :
```config

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

Attribution d'une adresse IPv4 sur une interface :
Pour les routeurs -> uniquement faisable sur des interfaces ou sous-interfaces physiques
Pour les switches -> uniquement faisable sur des interfaces vlan
```config
ip address 172.16.0.1 255.255.255.0
```

Attribution d'une adresse IPv6 sur une interface :
```config
ipv6 address 2001:db8:1::1/64
```

Ajout d'une description à l'interface :
```config
description Ceci est une description
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

## Sous-interface **Routeur Uniquement**
Entrer dans la sous-interface 10 du port GigabitEthernet0/0 :
```config
interface GigabitEthernet0/0.10
```

==Le numéro de vlan peut être différent du numéro de sous-interface==
Définition du vlan 10 sur la sous-interface :
```config
encapsulation dot1Q 10
```

Définition du vlan 90 en tant que vlan natif sur la sous-interface :
```config
encapsulation dot1Q 90 native
```

## Interface VLAN

Entrer dans l'interface du vlan 10 :
```config
interface vlan 10
```

# STP

Action du Rapid PVST+ :
```config
spanning-tree mode rapid-pvst
```

Passage du switch en root-bridge primaire pour les vlan 1,10,30,50,70 :
```config
spanning-tree vlan 1,10,30,50,70 root primary
```

Passage switch en root-bridge secondaire pour les vlan 1,10,20,30,40,50,60,70,80,99 :
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

# BGP
Afficher l'état de BGP :
```config
show ip bgp
```

Afficher le résumé de l'état de BGP :
```config
show ip bgp summary
```

Entrer dans la configuration de BGP pour l'AS 65001 :
```config
router bgp 65001
```

==Si le voisin est dans le même AS alors IBGP sinon EBGP==
Définition d'un voisin BGP avec l'adresse IP 1.1.1.1 appartenant à l'AS 65003 :
```config
neighbor 1.1.1.1 remote-as 65003
```

Définition du réseau 192.168.0.0/24 à redistribuer via BGP :
```config
network 192.168.0.0 mask 255.255.255.0
```

Activation de l'affichage des mis à jours des voisins BGP :
```config
bgp log-neighbor-changes
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