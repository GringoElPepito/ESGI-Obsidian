Les LXC représente la majorité des instances qui ont été déployés. Il y a plusieurs raisons à cela, la première est qu'il consomme beaucoup moins de ressources de calcul et de stockage qu'une VM. La seconde est qu'il se démarre plus rapidement que les VM. Cependant bien que les LXC possèdent certains avantages, ils ne sont pas exempt de tout défaut. Il n'est par exemple pas possible de réaliser de la migration à chaud sans interruption de service, un LXC utilisant directement le noyau de son hôte, il est impossible de transférer un LXC d'un hôte à un autre sans arrêter ce LXC.

Les LXC n'étant pas des systèmes à part entière, certaines configurations ne sont malheureusement pas réalisable (notamment celles affectant le noyau de l'instance ciblée)

Le déploiement des LXC est entièrement automatisé à travers le playbook deploy_lxc.yml. Voici comment ce dernier se découpe :

| Rôle                         | Action                                                                                                                       |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| lxc/check_if_exists          | Vérifie que le LXC n'existe pas déjà, si c'est le cas stoppe le playbook                                                     |
| lxc/select_ct_template       | Sélectionne la template qui servira de base pour le LXC                                                                      |
| lxc/create_ct_from_template  | Clone la template                                                                                                            |
| lxc/configure_ct             | Modifie la configuration du LXC                                                                                              |
| proxmox/add_pool_member      | Ajoute le LXC dans le ressource pool définit dans ses variables d'hôtes                                                      |
| proxmox/set_ha_resources     | Définit le LXC comme une ressource HA                                                                                        |
| common/start_instance        | Démarre le LXC                                                                                                               |
| common/reset_known_host      | Supprime l'enregistrement de l'IP dans le fichier known_hosts de l'hôte ansible                                              |
| common/get_facts             | Récupère les facts du LXC                                                                                                    |
| common/update_packages       | Mets à jour les paquets                                                                                                      |
| common/install_packages      | Installe les paquets définit dans les variables d'hôtes du LXC                                                               |
| lxc/hardening_ssh            | Pousse une configuration sécurisé de SSH et enregistre une clé publique pour les connexions suivante                         |
| common/set_certificate_facts | Initialisation des variables pour l'automatisation des tâches liés aux certificats                                           |
| common/get_certificates      | Récupère la chaîne de certificat et l'enregistre                                                                             |
| common/request_certificate   | Génère une demande de certificat, la fait signé par l'autorité intermédiaire et transmet le certificat au LXC                |
| common/request_certificate   | Génère une demande de certificat Post-Quantique, la fait signé par l'autorité intermédiaire et transmet le certificat au LXC |
| syslog/pki_files             | Place les fichiers de chiffrement aux emplacements dédiés pour le service Rsyslog                                            |
| syslog/push_conf             | Pousse la configuration Rsyslog                                                                                              |
| syslog/logrotate_conf        | Pousse la configuration de rotation de log                                                                                   |
| siem/agent                   | Installe l'agent Wazuh                                                                                                       |
| common/add_services_fwcmd    | Ajoute les services à firewalld                                                                                              |
| common/setsebool             | Ajoute les configurations booléennes pour SELinux                                                                            |
| common/sysctl                | Ajoute les configurations Sysctl                                                                                             |

Avec cette automatisation, il suffit de définir les variables d'hôtes nécessaire au playbook et de lancer ce dernier pour avoir un LXC fonctionnel et pré-configuré, permettant de gagner un certain temps dans le déploiement de nos services.