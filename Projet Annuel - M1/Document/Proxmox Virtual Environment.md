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
- Stockage :
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
- VxLAN : cette


La fonctionnalité **SDN de **Proxmox VE** permet de créer et gérer des réseaux virtuels de manière centralisée, sans avoir à configurer manuellement chaque interface réseau sur chaque nœud du cluster.

En d'autres termes, le SDN sépare la **configuration logique du réseau** de l'infrastructure physique. Cela est particulièrement utile dans les clusters Proxmox, les environnements multi-sites ou les laboratoires.