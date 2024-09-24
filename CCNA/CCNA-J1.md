r# Définition
Les réseaux
ISO institution international

# Model TCP/IP

| OSI          | TCP/IP         |
| ------------ | -------------- |
| Application  | Application    |
| Presentation | Application    |
| Session      | Application    |
| Transport    | Transport      |
| Network      | Internet       |
| DL           | Network Access |
| Physique     | Network Access |


### IPv4
les adresses IP sont séparés en classes, 32 bits réparties en 4 octets notation en décimal pointés. Il y a 2 parties dans l'adresse IP : Réseau / Host, on distingue ces 2 parties à l'aide du masque -> bit à 1 = Réseau / bit à 0 = Host.

#### Classe A
Pour les très grands réseaux.
Par défaut : 
- Réseau : 1er octet
- Host : 3 derniers octets
Masque : 255.0.0.0 -> /8 bitcount -> on compte le nombre de 1 dans le masque / CIDR = Classless InterDomain Routing.
Le 1er octet est compris entre 1 et 126
Il existe 126 réseaux de classe A chacun comporte 2²⁴ - 2 = 16 777 214 ID
on retire 2 adresses :
- La première : adresse réseau
- La dernière : adresse broadcast
#### Classe B
Pour les réseaux de tailles moyennes
Par défaut : 
- Réseau : 2 premiers octet
- Host : 2 derniers octets
Masque : 255.255.0.0 -> /16
Le 1er octet est compris entre 1 entre 128 et 191
Le second octet entre 0 et 255
Il existe 64 x 256 = 16384 réseaux de classe B
Chacun compte 2¹⁶ - 2 = 65535 IP

#### Classe C
Pour les réseaux de petites tailles
Par défaut : 
- Réseau : 3 premiers octet
- Host : le dernier octets
Masque : 255.255.255.0 -> /24
Le 1er octet est compris entre 1 entre 192 et 223
Le second octet entre 0 et 255
Il existe 64 x 256 x 256 = 2 097 152 réseaux de classe C
Chacun compte 2⁸ - 2 = 254 IP

#### Classe D
Pour le multicast (IP pour envoyer des flux à un groupe d'abonnés)
224.0.0.1 à 239.255.255.255

#### Classe E
Réservé (non utilisable)
240.0.0.0 à 255.255.255.254

#### IP localhost
127.X.Y.Z
ping localhost -> pile TCP/IP est fonctionnelle d'un point de vue protocolaire

### Subnetting
Conversion décimal / binaire
Un nombre est composée de chiffre. et est exprimé dans une base (binaire, hexa...)
Chaque chiffre a une place dans le nombre -> rang
base 10 ex -> 6⁴2³7²1¹3⁰ -> 6 x 10⁴ + 2 x 10³ + 7 x 10² + 1 x 10¹ + 3 x 10⁰

#### Conversion binaire / décimal
1⁷1⁶0⁵1⁴0³1²0¹1⁰ = 1 x 2⁷ + 1 x 2⁶ + 1 x 2⁴ + 1 x 2² + 1 x 2⁰
128 + 64 + 16 + 4 +1 = 213


187 - 128(2⁷) = 59
59 - 32 (2⁵) = 27
27 - 16 (2⁴) = 11
11 - 8 (2³) = 3
3 - 2 (2¹) = 1
1 - 1 (2⁰) = 0


| poids     | 1   | 2   |     |     |     |     |     |     |     |     |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Puissance | 0   | 1   |     |     |     |     |     |     |     |     |


#### Méthode
Soit le réseau 192.168.10.0/24 (255.255.255.0)
on veut des SR d'au moins 25 IP
1. On ajoute 2 au besoin (@Réseau & @Broadcast)
2. Combien de bits pour coder 27 possibilités -> 5 car 2⁵ = 32 ce qui est la valeur binaire égale ou supérieur à 27
3. Masque : on réserve 5 bits à 0 dans la partie classful (vers la gauche) avec des 1
255.255.255.224 = 1111 1111 . 1111 1111 . 1111 1111 . 1110 0000
pour le pas 256 - 224 = 32 -> les SR sont de 32 en 32 sur le 4e octet
IP directed Broadcast

### Routeur
équipement de couche 3 d'interconnexion réseau IP
composants : 
- CPU
- RAM : mémoire de travail, L'OS chargé le fichier de conf active, les tables et les caches files d'attente
- Flash : HD contient l'image de l'OS + d'autres fichier
- ROM : IOS minimaliste. Instruction Bootstrap
- NvRAM : Non volatile Random Access Memory -> fichier de configuration sauvegarder.
- interfaces : port de connexion

IOS Internetwork Operating System

Premier démarrage => Routeur vierge => mode Setup
Configuration initial => obligatoirement par le port console (RS232). => Nécessite accès physique.
Une fois le routeur configuré et accessible par le réseau IP
VTY = Virtual Teletype => SSH, Telnet

lien auxiliaire => on y branche un modem analogique liés à 1 prise téléphonique => associés à un numéro de téléphone => à distance de se connecter au matériel

Configuration -> ligne de commande -> CLI Command Line Interface
A la connexion = mode user => prompt : `router>` | accès limité au commande de visualisation.
Accès au commande pour des tests de base (ping, traceroute).

Mode privilégié -> `router>enable` => `router#` toutes les commandes de visualisation, tests avancés, commandes de dépannage (debug) -> voir en temps réel ce que fait le matériel.

Mode configuration global : `router# configure terminal` => `router(config)#` Modifier "parameters that affect the device as a whole".

Mode de configuration spécifique :
- interface
- protocole routage
- politique QoS
- etc...

Système d'aide :
- Complétion (touche tab) -> termine une directive s'il n'y a pas d’ambiguïté
- auto-complétion -> "raccourcir" les commandes s'il n'y a pas d'ambiguïté
- ? -> directive ? : toutes les options possibles après cette directive ; direc? -> affiche la complétion possible de la commande.

Changer hostname : `hostname NOUVEAU_NOM`

Configurer les mots de passe :
- Console : accéder à la console 
	1. `router(config)#line console 0`
	2. `#password MDP`
	3. `login` <- active l'authentification
- VTY (Telnet,SSH) : accéder à la console à distance
	1. `vty line vty 0 15` <- permet 16 connexions pour administration à distance
	2. `password MDP`
	3. `login`
- MDP privilégié : passer en mode privilégié, il est obligatoire pour accéder au mode privilégié depuis une session distante
	1. `(config)#enable secret MDP` <- mot de passe automatiquement chiffré dans le fichier de conf (MD5).

Password Encryption :
Hormis le mot de passe privilégié, les autres mot de passes ne sont pas chiffrés dans le fichier de conf.
`(config)#service password encryption`

Voir fichier de configuration : `show running-configuration`

Configuration d'une bannière : message s'affichant à la CLI du matériel. ```
```cisco-CLI
(config)#banner motd $ <- escape character
info
info
info$
```

Configurer les interfaces : 
```cisco-CLI
(config)#int type slot/sous-slot/port ex int e0/0
(config-if)#ip address 192.168.10.254 255.255.255.0
(config-if)#desc "LAN Site1"
(config-if)#no shut
(config-if)#int serial 1/0
(config-if)#ip addr 1.1.1.1 255.255.255.252
(config-if)#desc "Lien vers Paris"
(config-if)#no shut
```
