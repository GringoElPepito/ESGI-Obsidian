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