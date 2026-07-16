La virtualisation est un des éléments centraux de toutes infrastructures modernes. Là ou à l'époque chaque application devait fonctionner sur une machine distincte, la virtualisation a permis de faire tourner plusieurs applications sur un même serveur offrant ainsi la possibilité d'exploiter pleinement les ressources proposés par les serveurs physiques.
Il existe 2 de type de virtualisation :
- Virtualisation type 1 : la solution de virtualisation est intégré directement dans le noyau du système hôte (Ex: Proxmox, VMWare ESXi, Nutanix etc...).
- Virtualisation type 2 : la solution de virtualisation est une application externe installé sur le système d'exploitation existant (Ex: Virtualbox, VMWare Workstation etc...)
Comme expliqué précédemment lors de la section concernant les choix technologiques, nous nous sommes arrêtés sur la solution Proxmox Virtual Environment pour la virtualisation. Bien que plus récente, cette solution Open-Source commence à faire ses preuves mêmes dans les entreprises avec des contextes critiques. L'écart avec des solutions comme VMWare ESXi se réduit peu à peu au fil des mis à jour apportant de nouvelles fonctionnalités déjà présentes chez ses concurrents.
Par ailleurs Proxmox possède certaines fonctionnalités qui ne sont pas présentes chez d'autres solutions

Le cluster Proxmox se compose de 3 machines pour pouvoir respecter le quorum,
Voici les caractéristiques de la machine pve01.cenexis.lan :
- CPU : 
- RAM :
- OS : 
- Stockage :
- Interface réseau :
	- 


Proxmox est une solution offrant de nombreuses fonctionnalités qu'il est important de configurer judicieusement pour pouvoir pleinement en profiter.

## SDN
Pour la gestion des réseaux au sein du cluster Proxmox, nous avons utilisé la fonctionnalité SDN (Software- Defined Network) inclut par défaut dans Proxmox. 
La solution SDN de Proxmox permet de créer et gérer des réseaux virtuels de manière centralisée depuis la WebUI fournit par Proxmox. 
Une fois la configuration terminé, celle-ci est appliqué automatiquement aux nœuds ciblés par la configuration (par défaut l'ensemble du cluster), permettant de garantir une configuration uniforme d'une machine à l'autre. 
Pour mettre en place la fonctionnalité sur un cluster Proxmox, il faut dans un premier temps créer une Zone. 
C'est elle qui va définir le type de réseau virtuel qui pourra être utilisé. Il existe 5 types :
- Simple : un réseau local classique, cette zone n'est pas partagé entre les machines du cluster et ne permet donc pas la communication entre 2 instances (VM et/ou LXC) présentes sur 2 nœuds différents au sein du cluster.
- VLAN : cette zone implémente la norme IEEE 802.1Q et permet donc de segmenter le trafic avec les tags de VLAN. Cette zone est partagé et permet donc la communication entre 2 instances présentes sur des nœuds du cluster différents.
- QinQ : cette zone implémente la norme IEEE 802.1ad et permet d'insérer plusieurs tags VLAN au sein d'une même trames. Cette zone est partagé et permet donc la communication entre 2 instances présentes sur des nœuds du cluster différents.
- VxLAN : cette zone implémente la solution VxLAN et permet donc de créer un réseau de niveau 2 au dessus d'un réseau IP. Cette zone est partagé et permet donc la communication entre 2 instances présentes sur des nœuds du cluster différents.
- EVPN : cette zone est une version étendue du type VxLAN ajoutant un plan de contrôle BGP, particulièrement adapté aux grandes infrastructures. Cette zone est partagé et permet donc la communication entre 2 instances présentes sur des nœuds du cluster différents.
Les zones VLAN, QinQ, VxLAN et EVPN implémente des protocoles standards et peuvent donc servir de base pour étendre le réseau du cluster au-delà de ce dernier en faisant directement le pont avec l'infrastructure physique de manière transparente sans surcouche supplémentaires.
Le type de zone le plus adapté à notre besoin est le type VLAN, notre choix c'est donc arrêté sur cette option. Nous avons donc créer une zone appeler cenexis qui accueillera tous les sous-réseaux nécessaire aux fonctionnement de l'infrastructure. 
{ screen - ZONES PVE WebUI}
Une fois la zone créée, il faut désormais créer les sous-réseaux. Au sein de SDN les sous-réseaux sont appelés des VNets (Virtual Networks), dans le cas d'une zone de type VLAN, chaque VNet correspond à un VLAN distinct et à ce titre doit avoir un ID Vlan (tag) unique.
Concernant la nomenclature des VNet au sein du SDN, nous sommes limités à 8 caractères c'est pourquoi nous avons sélectionné la nomenclature suivante : VLAN{TAG_VLAN}. La norme 802.1q définit 4096 comme valeur maximal pour un tag VLAN de ce fait, celui-ci ne peut excéder 4 caractères et permet avec la nomenclature proposé de garantir qu'aucun troncage ne sera nécessaire pour le nommage des VNets. 
Cependant la nomenclature n'est pas très parlante et rend difficile d'identifier le type de trafic circulant sur chaque VNet, la solution à cela est de définir un Alias, l'alias n'est pas utilisé par Proxmox pour identifier les VLANs et est donc de ce fait bien plus permissif que le nom des VNets.
{ screen - VNets PVE WebUI }
La zone ainsi que l'entierté des VNets sont déployés via le playbook Ansible de configuration des instances Proxmox ainsi, ils nous suffit d'ajouter les informations des VLANs manquants au sein de nos fichiers de variables Ansible et de relancer le playbook pour les voir s'ajouter aux VLANs déjà existants.
Chaque VNet créé ajoutera une carte réseau virtuelle qui sera utilisable pour connecter nos instances LXC ou VM. Lorsqu'une instance VM ou LXC transmet une trame sur une carte réseau virtuelle de l'un des VNets de la zone de type VLAN, la trame est automatiquement étiquetée avec l'ID VLAN définit pour le VNet auquel l'instance est connecté, permettant d'identifier le VLAN d'origine pour les machines et systèmes amenés à traiter la trame.

## Haute disponibilité et Clusterisation

Proxmox offre la possibilité de clusteriser plusieurs machines PVE dans le but d'offrir un gestion centraliser de l'ensemble des membres du cluster et ceux sans avoir à installer d'instance virtuelle comme pour VMWare ESXi avec vSphere.
Lors de la mise en place d'un service en cluster, dans notre cas, Proxmox, il est indispensable de définir le quorum.Le principe de quorum permet de définir le nombre maximum d'instance pouvant rencontrer une panne sans endiguer le bon fonctionnement du service en cluster. 
On calcule le quorum à l'aide de la formule suivante : N/2+1, où N représente le nombre d'instance du cluster, la division est de N par 2 est une division euclidienne dont le résultat est donc un nombre entier. 
Dans notre cas, le cluster Proxmox est constitué de 3 machines, donc 3/2 + 1 = 1 + 1 = 2. Il nous faut de ce fait 2 machines opérationnelles pour que le cluster soit maintenu, ce qui signifie qu'au-delà d'une panne le cluster est considérer hors-service.

Une fois un cluster respectant le quorum installé, plusieurs fonctionnalités deviennent utilisables (certaines fonctionnalités requiert des éléments supplémentaires pour fonctionner). 

La fonctionnalité **High Availability (HA)** de Proxmox VE permet d'assurer la continuité de fonctionnement des machines virtuelles (VM) et des conteneurs (LXC) en cas de défaillance d'un serveur du cluster.

Le principe repose sur la surveillance permanente de l'état des nœuds du cluster grâce au système de quorum de Proxmox. Lorsqu'une machine virtuelle est déclarée comme ressource HA, son état est contrôlé en permanence. Si le serveur qui l'héberge devient indisponible (panne matérielle, coupure d'alimentation, perte de connexion au cluster, etc.), le gestionnaire HA détecte automatiquement cette défaillance.

Si le quorum du cluster est maintenu et que les ressources nécessaires sont disponibles, la VM est automatiquement redémarrée sur un autre nœud du cluster. Cette opération ne constitue pas une migration à chaud : la machine est arrêtée de manière forcée sur le nœud défaillant puis redémarrée sur un autre serveur à partir de son stockage partagé ou répliqué.

Le système HA s'appuie principalement sur deux composants :
- **Le CRM (Cluster Resource Manager)**, chargé de décider sur quel nœud chaque ressource doit être exécutée.
- **Le LRM (Local Resource Manager)**, présent sur chaque nœud, qui applique localement les décisions du CRM.

L'utilisation de la haute disponibilité nécessite généralement :
- un cluster Proxmox fonctionnel avec quorum ;
- un stockage accessible par plusieurs nœuds (par exemple Ceph, NFS, iSCSI ou ZFS avec réplication adaptée) ;
- des ressources suffisantes sur les serveurs capables de reprendre les charges de travail en cas de panne.

La haute disponibilité améliore significativement la résilience de l'infrastructure en réduisant le temps d'interruption des services hébergés.

Le **Cluster Resource Scheduling (CRS)** est un mécanisme permettant d'optimiser automatiquement la répartition des machines virtuelles et des conteneurs au sein d'un cluster Proxmox.

Contrairement au système HA, dont l'objectif est de gérer les pannes, le CRS intervient lorsque le cluster fonctionne normalement. Il analyse régulièrement l'utilisation des ressources (processeur, mémoire, charge des nœuds, etc.) afin d'équilibrer les charges entre les différents serveurs.

Lorsque cela est possible, le CRS peut proposer ou effectuer des migrations à chaud (_Live Migration_) afin de déplacer certaines VM vers des nœuds moins sollicités. Cette répartition permet de :

- limiter la surcharge d'un serveur ;
- améliorer les performances globales du cluster ;
- optimiser l'utilisation des ressources matérielles ;
- préparer plus facilement les opérations de maintenance.

Le CRS peut fonctionner selon différentes politiques de placement, prenant en compte la disponibilité des ressources et les contraintes définies par l'administrateur.

L'objectif est d'obtenir une infrastructure plus équilibrée sans intervention manuelle permanente.

---

## Règles d'affinité (Affinity Rules)

Les **Affinity Rules** permettent de contrôler le placement des machines virtuelles à l'intérieur du cluster en définissant des relations entre elles ou avec certains nœuds.

Ces règles offrent une plus grande maîtrise de l'organisation des services tout en restant compatibles avec les mécanismes de haute disponibilité et de planification des ressources.

Deux types principaux de règles existent.

### Règles d'affinité (Affinity)

Les règles d'affinité imposent que plusieurs machines virtuelles soient hébergées sur le même nœud lorsque cela est possible.

Cette configuration est utile lorsque plusieurs services communiquent très fréquemment, par exemple :
- un serveur web et son cache ;
- une application et son serveur Redis ;
- plusieurs composants d'une même application distribuée.

Le fait de les exécuter sur le même hôte permet de réduire la latence réseau et d'améliorer les performances.

### Règles d'anti-affinité (Anti-Affinity)
À l'inverse, les règles d'anti-affinité imposent que certaines machines virtuelles ne soient jamais hébergées sur le même serveur.

Cette approche est particulièrement utilisée pour les services critiques redondants, par exemple :

- deux contrôleurs de domaine ;
- plusieurs serveurs de bases de données répliquées ;
- plusieurs nœuds Kubernetes ;
- des pare-feux virtuels fonctionnant en haute disponibilité.

En répartissant ces machines sur des hôtes différents, une panne matérielle n'affectera pas simultanément tous les composants du service.

---

## Complémentarité des trois fonctionnalités

La haute disponibilité, le Cluster Resource Scheduling et les règles d'affinité répondent à des objectifs complémentaires.

La **HA** assure la reprise automatique des services après une défaillance d'un nœud. Le **CRS** optimise la répartition des charges lorsque le cluster fonctionne normalement en équilibrant les ressources disponibles. Enfin, les **Affinity Rules** permettent à l'administrateur de définir des contraintes de placement afin de rapprocher ou d'éloigner certaines machines virtuelles selon les besoins fonctionnels ou les exigences de disponibilité.

Combinées, ces fonctionnalités permettent de construire une infrastructure virtualisée à la fois performante, résiliente et facilement administrable, tout en garantissant une meilleure exploitation des ressources matérielles du cluster.