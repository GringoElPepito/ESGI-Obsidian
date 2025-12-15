# Introduction
## Avant la virtu :
Architecture : 1 serveur physique pour 1 système d'exploitation et une application, 
résultat de cette architecture :
- Gaspillage des ressources (RAM, CPU, Stockage) : 5 à 10% de la capacité de la machine physique réellement utilisé.
- Temps de déploiement et de mise en prod bien plus long, étape nécessaire :
	- Commande du serveur
	- Installation et MAJ de l'OS
	- Configuration du système
	- Installation et configuration de l'application
- Souvent nécessaire d'interrompre les services des applications, les maintenances sur le serveur physique pouvait demander un arrêt complet de la machine.
- Grand espace physique nécessaire au stockage des serveurs physique dans les baies de rackage
- Consommation électrique élevée
## Avec la virtu :
Architecture : 1 serveur, pour X systèmes d'exploitation et X application
résultat de cette architecture :
- Consolidation des charges de travail et des ressources, il est maintenant possible d'exploiter pleinement les capacités de chaque serveur physique, jusqu'à 80% sans perte de performances pour les machines virtuelles.
- Gains d'espace et d'énergie, moins de serveurs physiques nécessaires pour faire fonctionner le même nombre de services.
- Gain de temps et d'argent, déploiement et mise en production accéléré, création d'une VM en quelques minutes depuis l'interface graphique.
- Maintenance de l'hyperviseur sans interruption de service, possibilité de réaliser des migrations à chaud (sans arrêt de la VM) pour répartir la charge d'un hyperviseur entre tous les autres et pouvoir assurer la maintenance de celui-ci sans problème de maintien du service.

## Qu'est-ce qu'un hyperviseur ? :
Un hyperviseur permet de mutualiser les ressources d'une architecture matérielle physique en plusieurs architectures matérielles logiques. En d'autres termes, c'est une solution logicielle permettant de créer et d'exécuter des machines virtuelles. Virtual Machine Monitor (VMM) est un alias d'hyperviseur.
L'une des fonctions principales d'un hyperviseur est l'isolation, une machine virtuelle ne peut pas affecter l'hôte ou d'autres machines virtuelles, même en cas de blocage.

Schéma de base d'un ESXi :
- 4 ressources matérielles :
	- 3 (CPU, RAM, Réseau)
	- 1 (Stockage)
- Couche de virtualisation (ESXi) OS propriétaire
- Machines virtuelles utilisant les ressources de la machine hôte, pour faire fonctionner des services/applications.

Hyperviseur de type 1 (Hyperviseur natif ou bare métal) ->Il s'exécute directement sur le matériel physique, les machines virtuelles s'exécutent directement sur l'hyperviseur. (Ex: VM Oracle, VMWare ESXi, Hyper-V)

Hyperviseur de type 2 -> Il est installé en tant qu'application sur un système d'exploitation hôte existant. (Ex: VirtualBox, MS Virtual PC, VMWare Server et Workstation)

# Gestion Stockage et Réseau :
## Réseau :
Découpage Réseau chez VMWare ESXi :
1. Il y a tout d'abord les Switches physiques qui transmettent le réseau
2. Chaque ESXi possèdent des ==vmnic== qui sont les cartes réseaux physiques reliés aux commutateurs (Switches) physiques
3. Chaque vmnic est relié à un ==vSwitch==, qui est un switch virtuel présent au sein de l'ESXi. une vmnic ne peut être associé qu'à un vSwitch et un vSwitch peut être associé à plusieurs vmnic.
4. Pour chaque vSwitch, on peut créer des ==Portgroups== (Groupe de ports). Il est possible d'attribuer des VLANs à un Portgroup
5. Les cartes réseaux des VM sont connectés à un Portgroup de l'ESXi. Une carte réseau d'une VM ne peut être associé qu'à un seul Portgroup. Pour qu'une VM ait accès à plusieurs Portgroup, il faudra donc lui ajouter d'autres carte réseau.

Il existe 2 types de Portgroup :
- ==Type VM== dédié au trafic des VM
- ==Type VMkernel== dédié aux différents services des ESXi (Management, vMotion, vSAN, Fault Tolerance). Chaque Portgroup de type VMkernel doit avoir une IP statique.

2 configurations de port pour les switches physiques :
- Access -> ne laisse passer qu'un seul VLAN
- Trunk -> laisse passer plusieurs VLAN à la fois

Composant d'un vSwitch standard :
1. Connexion de machines virtuelles au réseau physique. (Portgroup de type VM)
2. Connexion des services VMkernel au réseau physique. (Portgroup de type VMkernel).

Pour que les Portgroup VMkernel et VM puisse accéder au réseau extérieur, il faut que des cartes réseaux de l'ESXi (Uplink Ports) soient assignés au vSwitch portant ces Portgroups, ce qui n'est pas forcément le cas. Il est possible d'avoir un vSwitch connecté à aucune vmnic permettant ainsi la communication uniquement entre les machines virtuelles.

On peut soit répartir notre réseau ESXi entre différent vSwitch soit tout mutualiser sur un seul unique vSwitch et segmenter les différents trafics via des Portgroups. Dans le second cas, tous les Portgroups passeront pas le ou les mêmes ports physiques pour sortir (Possible via trunk).


## Stockage :
Local Storage est un Datastore accessible par un seul et unique ESXi. On peut installé des VM dessus mais ne permet pas la HA, vMotion ou encore DRS. Bien pour test mais pas pour prod

Shared Storage est un Datastore accessible par plusieurs ESXi permettant la HA, vMotion ou encore DRS. Il peut se présenter sous la forme d'une baie SAN ou NAS ou encore sous une solution virtualisé comme vSAN.

Datastore est un conteneur contenant des fichiers et des objets. Ils sont des conteneurs logiques indépendant du système de fichier permettant un modèle uniforme pour le stockage des fichiers de machine virtuelle.

Systèmes de fichier pris en charge par vSphere :
- VMFS
- NFS
- Virtual SAN
- Virtual Volumes

Une machine virtuelle est stockée sous la forme d'un ensemble de fichier dans un répertoire (VMFS/NFS) ou d'un groupe d'objets dans une banque de données (vSAN).

Les Datastores peuvent également stockés ISO, Template etc...

Type de provisionnement de stockage :
- Thin : ne consommera que l'espace réellement consommé par la VM
- Thick : consommera tout l'espace attribué à la VM même si pas réellement utilisé

# Découverte VM
Config MAX pour VM en 7.0 :
- 4 adaptateurs SCSI
- 15 disques par adaptateurs -> 60 disques MAX par VM
- 10 cartes réseau 
- 6To de RAM

Fichier d'une VM :
- vmx -> fichier de configuration, permet d'inscrire la VM dans l'inventaire. vmx devient vmtx lorsque la VM est convertit en template
- nvram -> BIOS de la VM
- log -> fichier journal
- vmdk -> Fichier description disque de la VM
- flat-vmdk -> disque virtuel de la machine
- vmss -> Etat de la VM en mode suspend
- vswp -> fichier swap de la VM, à prendre en compte pour le dimensionnement des datastores. utilisé pour une migration vMotion. Sa taille est égal à la quantité de RAM alloué à la VM. il est créé au démarrage et supprimé à l'arrêt de la VM.
- -delta.vmdk -> Fichier de données du snapshot
- vmsd -> métadonnées des snapshot
- vmsn -> état du snapshot de la VM

Suppression de l'inventaire -> les fichier de la VM sont toujours présents
Suppresion du disque -> les fichiers de la VM sont supprimés

VMWare tools :
- Possibilité d'arrêter "proprement" les VM
- Améliorations des performances graphiques
- Synchronisation du temps via l'utilisation des paramètres NTP de l'ESXi.

Les snapshots :
.vmsd contient des entrées de ligne définissant les relations entre les snapshots et les disques enfants de chaque snapshot.
.vmsn contient l'état de la VM, il est créé si la case prendre en compte la mémoire est coché. Il enregistre les informations contenu dans la RAM de la VM.
-Delat.vmdk delta des données d'un disque depuis le snapshot.

Possibilité de cloner une VM, pour éviter les conflits :
- Modifier hostname
- adressage IP
- adresse MAC
- Sysprep (Windows uniquement)
Possibilité de créer une VM à partir d'une template. gain de temps, standardisation des configurations etc...
une VM soit être éteinte pour être convertit en VM. Possible de cloner une VM en template sans que la machine soit éteinte.

# vMotion
deux type de migration vMotion :
- Migration à chaud (VM allumé)
- Migration à froid (VM éteinte)

3 possibilités de migration :
- Migration du compute (RAM, CPU, Réseau) -> Stockage partagé entre les deux ESXi nécessaire
- Migration du stockage
- Migration du compute et du storage

Prérequis :
- 2 hôtes ESXi ou plus
- Compatibilité CPU, minimum même fabricant
- 1 instance vCenter
- Un cluster vCenter
- Adressage IP statique pour les hôtes ESXi
- configuration réseau identique pour les ESXi avec un Portgroup VMkernel ayant l'option vMotion activé sur le même plan d'adressage
- Stockage partagé entre les hôtes
- Ressources disponible sur l'hôte cible

mode EVC :
Simplifie l'utilisation de vMotion en utilisant des processeurs de plusieurs générations. Ajoute une option pour choisir la génération du processeur

Migration à chaud :
- RAM migré sur le réseau vMotion de l'hôte source à l'hôte cible
- CPU déroulement en 2 temps :
	- Toutes les instructions CPU en cours d'exécutions continuent d'être envoyé au CPU physique de l'hôte source jusqu'à ce qu'il n'y en ait plus
	- Toutes les nouvelles instructions CPU demandés par la VM sont envoyé au CPU de l'hôte cible
- Réseau routage des paquets une fois migration terminée

Une fois ces étapes achevées, l'hyperviseur destination prend la main sur les fichiers.

Cause d'échec connexion à un périphérique virtuelle, lecteur CD/DVD ou disquette, ou image présent uniquement localement sur l'hôte source.

Dans le cas d'une migration stockage et compute, le stockage est migré en premier.

# HA
permet de se protéger contre :
- panne d'un ESXi, si un hôte ESXi est en panne alors les VM redémarreront sur les autres hôtes du cluster
- panne d'une VM, si le monitoring est activé sur la VM et que la VM stop l'envoie de heartbeats à vCenter la VM est reset et reste sur le même hôte

Prérequis :
- 2 hôtes ESXi ou plus
- 1 instance vCenter
- Un cluster vCenter
- adressage IP statique des hôtes ESXi
- Configuration réseau identique pour les ESXi
- Stockage partagé entre les hôtes ESXi

Il y a 2 rôle dans un cluster HA :
- Master ou Maître
- Slave ou esclave

Ils sont attribué par une élection se déroulant à l'activation de la fonctionnalité HA.
L'ESXi avec le plus de datastore est élu maitre. Si égalité, l'hôte avec le plus grand MOID (id aléatoire généré à l'installation de l'ESXi) est élu Maître.

Si le maître est défaillant une nouvelle élection à lieu.

Le Master communique avec vCenter et les ESXi slaves. Le master doit déterminé l'état de chaque slave (Panne total ou isolement réseau).
En cas d'isolement réseau, le comportement est déterminé par la configuration, les VM ne sont pas systématiquement déplacés vers les autres ESXi, car seul le réseau management peut être impacté et non les autres réseaux, les VM pouvant donc être potentiellement joignables.

A l'ajout d'un ESXi au cluster ou à l'activation de la fonctionnalité, un agent FDM est installé sur les ESXi du cluster.

L'agent FDM du Master communique avec les agents FDM des slave ainsi qu'avec vCenter.

Si le master tombe vCenter déclenche une nouvel élection en parallèle se fait le redémarrage des VM sur les autres ESXi.

2 types de pannes :
- PDL : Permanent Device Loss (Arrêt électrique de l'hôte ou coupure total du réseau) - perte d'accessibilité irrécupérable d'un datastore, il est possible de mettre hors tension les VM pour les redémarrer sur un autre ESXi
- APD : All Path Down (Coupure partiel du réseau - réseau management) - perte d'accessibilité temporaire ou inconnue ayant un retard dans les traitements des E/S, cette condition peut être rétablie. Possible de ne pas agir sur les VM

Pour déterminer le type de panne un ESXi slave communique avec :
- l'ESXi master sur le réseau management
- les datastores de sont cluster
- la ou les gateways configuré au niveau HA

Contrôle d'admission, s'assure qu'il y a assez de ressources (CPU et RAM) pour les VM du cluster en cas de panne d'un ESXi.

Nombre d'échec hôte toléré :
Si valide cela veut dire que même si un ESXi tombe, les hôtes restant pourront supportés la charge supplémentaire.

Si non valide, alors les VM ne pourront pas être démarrés afin de respecter la règle du contrôle d'admission

Pourcentage de ressource à réservées :
