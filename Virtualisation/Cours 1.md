Modèle avant virtualisation :
![[Pasted image 20241021160232.png]]
Inconvénients :
1. Gaspillage des ressources. 5 à 10% des ressources serveurs réellement utilisés
2. Temps déploiement beaucoup trop long.
3. Obligation de faire des interruptions de serveurs, moins de flexibilités notamment pour la Haute Disponibilité.
4. Espace physique nécessaire dans les datacenters car une application par serveur.
5. Très haute consommation électrique.

Grâce à la virtualisation :
1. Possibilité de monté jusqu'à 90% des ressources physiques du serveur
2. Accélération du temps de déploiement, notamment grâce au template
3. Possibilité de réduire voire nullifier les interruptions de services
4. Réduction de l'espace nécessaire, car moins de serveurs physiques
5. La consommation est donc réduite.
Une machine virtuelle est un ensemble de fichier comportant un système d'exploitation. Chaque machine est en concurrence dans l'accès aux ressources de la machine physique. Il est possible de dimensionner les ressources allouer à chacune des machines virtuelles.
Un hyperviseur est un logiciel permettant de créer et d'exécuter une ou plusieurs VM. aussi appelé moniteur

Hyperviseur type 1 = hyperviseur natif ou bare metal
Hyperviseur type 2 = hyperviseur hébergé. ce type d'hyperviseur est installé en tant qu'application logicielle sur un système d'exploitation hôte existant. VirtualBox, Microsoft Virtual PC, VMWare Workstation.

Prérequis Matériel :
- CPU
	- Minimum 2 coeur 
	- Activé virtualisation
- RAM
	- 4Go de RAM
- Disque
	- 32Go de stockage
- Réseau
	- 1 carte réseau minimum, plusieurs pour scinder les différents trafic

# Modèle d'architecture virtualisation

|              | On-Premise                                            | Cloud                                                                    | Hybrid                              |
| ------------ | ----------------------------------------------------- | ------------------------------------------------------------------------ | ----------------------------------- |
| Définition   | Infra. Hébergé sur site gérée et maintenue en interne | Infra. Hébergée par un fournisseur de cloud accessible via interface web | Combinaison des deux environnements |
| Coût initial | Coûts élevés achat matérielle,                        |                                                                          |                                     |
|              |                                                       |                                                                          |                                     |
|              |                                                       |                                                                          |                                     |
|              |                                                       |                                                                          |                                     |
# Lien entre conteneurisation et virtualisation
