# Utilisation des ressources
Top/htop (h pour humain) permet de voir
- La charge CPU
- La consommation de RAM
- Les disques 
- Les processus

`ps -aux` permet de voir les processus en cours sur la machine
Option de `ps` :
- a : pour les services exécutés par tous les autres utilisateurs
- u : pour les services exécutés par l'utilisateur connecté
- x : pour les processus fonctionnant en arrière-plan

`uptime` permet de voir depuis combien de temps la machine est allumé

# Réseau
`arp` permet d'accéder à la table CAM de la machine

`ping` permet de réaliser un test de connectivité sur une machine distante et de faire du Port Knocking, qui est option permettant d'activer dynamiquement un service ou déclenché une action en envoyant un séquence de ping précise avec le bon nombre de paquet et le bon intervalle

`traceroute` -> permet d'afficher la route utilisé pour atteindre une destination
`mtr` -> permet d'afficher la route utilisé pour atteindre une destination avec modification en temps réel
`tcptraceroute` -> permet d'afficher la route utilisé pour atteindre une destination via une communication TCP/IP

`route add -net IP/CIDR gw GATEWAY` -> permet d'ajouter une route réseau 

`route del -net IP/CIDR gw GATEWAY` -> permet de supprimer une route réseau

`nmap` -> permet de vérifier les ports ouvert par une machine

`tcpdump` -> permet d'afficher l'intégralité des segments entrants

`apt` -> package manager de Distrib basé sur Debian
`yum/dnf` -> package manager de Distrib basé sur RedHat

Pour RedHat
- `enabled_metada` -> Permet d'activer la récupération des métadonnées du Repository
- `sslverify` -> Permet d'activer ou non la vérification du certificat du Repository
- `gpgkey` -> Permet de préciser l'emplacement de la clé GPG permettant de vérifier la signature des paquets
- `baseurl` -> Lien http ou https du Repository