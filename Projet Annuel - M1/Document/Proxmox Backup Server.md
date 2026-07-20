La sauvegarde est l'un des éléments principaux de sécurité pour une entreprise. En effet, si l'entreprise venait à perdre ses données cela pourrait avoir un impact terrible sur la production qui dans la plupart des cas devra être stoppé de toute urgence. C'est pourquoi il est important de mettre en place un cycle de sauvegarde complet.

Notre solution de virtualisation étant Proxmox Virtual Environment, nous avons choisi Proxmox Backup Sever comme solution de sauvegarde.

Voici les caractéristiques du serveur de sauvegarde fty-bck01.cenexis.lan :

| Paramètres      | Valeur                       |
| --------------- | ---------------------------- |
| Type d'instance | Serveur physique             |
| OS              | Proxmox Backup Server 4.2.2  |
| CPU             | 4 CPU                        |
| RAM             | 16G                          |
| Stockage        | 1To                          |
| Interfaces      | vmbr0: VLAN150 -> 10.15.0.10 |

**Proxmox Backup Server (PBS)** est une solution de sauvegarde développée par Proxmox pour protéger les machines virtuelles, les conteneurs LXC et les hôtes Proxmox VE. Conçue pour s'intégrer nativement à Proxmox VE, elle permet de centraliser la gestion des sauvegardes et de simplifier les opérations de restauration.

PBS utilise un mécanisme semblable de sauvegarde **incrémentielle** basé sur la **déduplication des données**. Lorsqu'une nouvelle sauvegarde est effectuée, seules les données modifiées depuis la précédente sont transférées et stockées. Cette approche réduit considérablement l'espace de stockage nécessaire ainsi que le temps requis pour réaliser les sauvegardes.

La solution intègre également des fonctionnalités telles que le chiffrement des sauvegardes, la vérification de leur intégrité, la compression des données et la planification automatique des tâches. En cas de perte ou de défaillance d'une machine virtuelle ou d'un conteneur, Proxmox Backup Server permet de restaurer rapidement les données afin de garantir la continuité des services.

Une partie de la configuration de Proxmox Backup Server est automatisé via le playbook deploy_proxmox.yml :

| Rôle                  | Action                                     |
| --------------------- | ------------------------------------------ |
| proxmox/pbs_zfs_disk  | Créer le pool zfs                          |
| proxmox/add_pbs_users | Créer les utilisateurs PAM et PBS          |
| proxmox/connect_pbs   | Connecte le serveur PBS au Cluster Proxmox |

# Exploitation

## Ports en écoute
