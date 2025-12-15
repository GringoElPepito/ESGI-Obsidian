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

Possibilité de cloner une VM