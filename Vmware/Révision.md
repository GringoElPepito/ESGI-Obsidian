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
- Gains d'espace et d'énergie, moins de serveurs physiques nécessaires pour faire fonctionner le même nombre de services