# Présentation

Dans une infrastructure hautement disponible, le choix du stockage est un point primordial qui doit être attentivement choisi pour répondre au mieux aux besoins du projet. 
La solution retenu définira certaines contraintes qui devront être respecté pour garantir le bon fonctionnement de l'infrastructure.
Nous souhaitions initialement nous orienté vers Ceph qui est une solution de stockage hyperconvergé, c'est à dire que chaque instance du cluster de virtualisation participe au stockage en mettant à disposition ses disques locaux aux restes du cluster. 
Bien que présentant de nombreux avantages, Ceph s'accompagne de son lot d'inconvénients, le premier est qu'il requiert une assez grande quantité de disque pour garantir le fonctionnement du cluster en cas de panne. 
Le second est qu'une grande partie des disques alloués est rendu inutilisable à cause de la duplication de données réalisé dans le but de maintenir le service opérationnel en cas de panne. 
Enfin le troisième inconvénient est la consommation de ressources supplémentaires nécessaire au fonctionnement de Ceph.

Pour les raisons citées plutôt, nous avons finalement décidés de nous orientés vers une solution de stockage SAN (Storage Area Network). 
Avec du stockage SAN, on va dédié des machines aux stockages et connectés les noeuds de virtualisation à ces machines avec une connexion dédié, pour que ces derniers puissent accéder aux stockages mis à disposition par le serveurs de stockage. à savoir TrueNAS SCALE. 

Voici les caractéristiques de notre machine TrueNAS :
-          CPU : 2 cores
-          RAM : 4Gib
-          Nom d’hôte : san.kasgrunt.lan
-          IP : 172.21.51.59
-          Stockage :
o   Disque de 32Gb pour l’installation du système d’exploitation
o   Disque de 100Gb
o   Disque de 3Tb

Voici les caractéristiques de fty-lsto01.cenexis.lan :

| Paramètres      | Valeur                         |
| --------------- | ------------------------------ |
| Type d'instance | Serveur Physique               |
| OS              | TrueNAS SCALE                  |
| CPU             | 6 CPU                          |
| RAM             | 16G                            |
| Stockage        | - 128Go OS<br>- 128Go<br>- 1To |
| Interfaces      | eth0: VLAN161 -> 10.16.1.20    |
# Installation

Nous avons configuré sur celle-ci deux pools de stockage :

-          ISO_POOL, un espace utilisant le disque de 100G servant à entreposer les ISO d’installation nécessaire à la création des machines virtuelles.
{ screen - ISO POOL }

-          VM_POOL, un espace utilisant le disque de 3Tb dédié au stockage des machines virtuelles.
{ screen - VM_POOL }

ISO_POOL est partagé en NFS, tandis que VM_POOL l’est via iSCSI. Nous avons fait ce choix car iSCSI permet à la machine distante accédant au stockage de directement modifié les blocs offrant des biens meilleurs performances. Cependant, iSCSI n’intègre pas de système de résolution de conflit, dans le cas des machines virtuelles cela n’est pas un problème car les données de chaque machine virtuelle n’est utilisé que par l’un des trois serveurs Proxmox à la fois. Mais ceci n’est pas vrai pour les ISO d’installation, en effet, il est très probable que les trois PVE souhaitent accéder à un même fichier ISO au même moment, ce qui pourrait se résulter en un conflit si on utilisait le protocole iSCSI. C’est pourquoi les ISO sont partagé grâce à NFS, ce protocole intègre directement un système de résolution de conflit répondant directement au problème posé par notre cas d’utilisation.

Pour le partage iSCSI, nous avons configuré un dataset nommé vm-datastore. Les dataset sont des systèmes de fichier créer au sein d’un pool de stockage qui vont pouvoir ainsi être exploités pour stocker des fichiers et répertoires.
{ screen - dataset }

Nous avons aussi limité l’accès au partage iSCSI aux IQN (Nom Qualifié iSCSI) de nos PVE. De plus nous avons aussi créer deux utilisateurs dédiés à l’exploitation du partage iSCSI par les ESXi pour limiter l’impact d’une potentiel compromission des PVE
{ screen - Config iSCSI }

Concernant la configuration du partage NFS, celui-ci est monté dans le dossier /mnt qui est sur linux le dossier par défaut pour monter des volumes. De plus nous avons aussi restreint les accès au partage aux adresses IP des PVE permettant ainsi de réduire le risque d’une attaque ou d’une compromission de partage.

## Ports en écoute
TrueNAS est un système proposant de nombreuses fonctionnalités à ce titre, un certain nombre de port sont en écoute pour assurer le bon fonctionnement de chaque service. En voici la liste et le détail de chacun de ses ports :
- 22 : Accès SSH
- 80 : HTTP pour redirection vers HTTPS
- 443 : HTTPS
- 2049 : NFS
- 3260 : iSCSI
# Exploitation

## Accès 
Le première accès disponible est l'accès SSH rendu disponible à travers le bastion JumpServer (fty-lbst01.cenexis.lan), celui-ci sera principalement utilisé pour s'occuper de la gestion de l'instance. L'accès SSH permet de réaliser les mis à jour systèmes ou encore de débuguer les différents services, s'ils venaient à être hors-service. L'authentification utilise des clé SSH et est entièrement géré par le bastion.

Le second accès de cette machine se fait via l'interface web à laquelle on accèdera encore une fois à travers le bastion JumpServer. Cet accès permet notamment de configuré les différents services liés au fonctionnement de TrueNAS de manière plus intuitive que via l'accès SSH.
{ screen - TrueNAS WebUI }

## Gestion général

Concernant la gestion général, l'idéal est d'intervenir à travers l'interface pour sa facilité de prise en main. Les services iSCSI et NFS, sont les 2 services critiques nécessaires au bon fonctio


## Mise à jour
Pour mettre à jour la machine fty-lsto01.cenexis.lan, il faut au préalable couper les connexions entre celle-ci et les PVE. Ensuite depuis l'interface web il faut se rendre dans System -> Update -> Install Update et suivre les instructions.