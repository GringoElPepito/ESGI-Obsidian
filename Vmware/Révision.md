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
