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
Cependant la nomenclature n'est pas très parlante et rend difficile d'identifier le type de trafic circulant sur chaque VNet, la solution à cela est de définir un Alias, l'alias n'est pas utilisé par Proxmox pour identifier les VLANs et est donc de ce fait bien plus permissif que le nom des V