La virtualisation est un des éléments centraux de toutes infrastructures modernes. Là ou à l'époque chaque application devait fonctionner sur une machine distincte, la virtualisation a permis de faire tourner plusieurs applications sur un même serveur offrant ainsi la possibilité d'exploiter pleinement les ressources proposés par les serveurs physiques.
Il existe 2 de type de virtualisation :
- Virtualisation type 1 : la solution de virtualisation est intégré directement dans le noyau du système hôte (Ex: Proxmox, VMWare ESXi, Nutanix etc...).
- Virtualisation type 2 : la solution de virtualisation est une application externe installé sur le système d'exploitation existant (Ex: Virtualbox, VMWare Workstation etc...)
Comme expliqué précédemment lors de la section concernant les choix technologiques, nous nous sommes arrêtés sur la solution Proxmox Virtual Environment pour la virtualisation. Bien que plus récente, cette solution Open-Source commence à faire ses preuves mêmes dans les entreprises avec des contextes critiques. L'écart avec des solutions comme VMWare ESXi se réduit peu à peu au fil des mis à jour apportant de nouvelles fonctionnalités déjà présentes chez ses concurrents.
Par ailleurs Proxmox possède certaines fonctionnalités qui ne sont pas présentes chez d'autres solutions

Le cluster Proxmox se compose de 3 machines pour pouvoir respecter le quorum,
Voici les caractéristiques de la machine pve01.cenexis.lan :
- CPU : 
- RAM


Proxmox est une solution offrant de nombreuses fonctionnalités qu'il est important de configurer judicieusement pour pouvoir pleinement en profiter.

## SDN
