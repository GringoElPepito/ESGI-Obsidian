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
	1. `line vty 0 15` <- permet 16 connexions pour administration à distance
	2. `password MDP`
	3. `login`
	4. `transport input ssh` <- N'autorise que les connexions SSH
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
