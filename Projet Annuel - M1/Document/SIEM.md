La mise en place d'une solution de gestion des informations et des événements de sécurité (SIEM) est essentielle pour une entreprise exerçant une activité critique et traitant des données sensibles. En centralisant et en analysant en temps réel les journaux d'événements issus des différents équipements et applications, le SIEM permet de détecter rapidement les comportements anormaux, les tentatives d'intrusion et les incidents de sécurité. Il améliore la capacité de réaction des équipes, facilite les investigations grâce à la conservation des traces d'activité et contribue au respect des exigences réglementaires en matière de sécurité et de traçabilité. En offrant une visibilité globale sur l'état de sécurité du système d'information, le SIEM renforce la résilience de l'entreprise face aux cybermenaces.

Voici les caractéristiques de l'instance fty-lsie01.cenexis.lan :

| Paramètres      | Valeur                                                             |
| --------------- | ------------------------------------------------------------------ |
| Type d'instance | LXC                                                                |
| OS              | Rocky 10.2                                                         |
| CPU             | 2 vCPU                                                             |
| RAM             | 4G                                                                 |
| Stockage        | 64G                                                                |
| Interfaces      | eth0: VLAN161 -> 10.16.1.20                                        |

{screen - Summary fty-lsie01.cenexis.lan}
### Installation

Dans notre cas, nous avons optés pour la solution Wazuh qui est une solution SIEM Open-Source ayant à de nombreuses reprises fait ses preuves en proposant un système d'analyse complet allant au-delà de la simple analyse de logs. Notre installation de Wazuh est une installation All-in-One,  c'est à dire que l'ensemble des composants de la solution est installé sur un même serveur. Cette configuration regroupe le **Wazuh Server**, chargé de recevoir et d'analyser les événements de sécurité transmis par les agents, le **Wazuh Indexer**, qui stocke et indexe les données pour permettre leur recherche et leur corrélation, ainsi que le **Wazuh Dashboard**, qui fournit une interface web de supervision, de visualisation et d'administration. 
Les agents Wazuh, déployés sur les postes de travail, les serveurs et autres équipements compatibles, collectent les journaux système, surveillent l'intégrité des fichiers, détectent les vulnérabilités, contrôlent la conformité des configurations et remontent les événements de sécurité vers le serveur. Les règles de détection intégrées permettent d'identifier les comportements suspects et de générer des alertes en temps réel. Cette architecture centralisée simplifie le déploiement, l'administration et la maintenance de la plateforme, tout en offrant une visibilité complète sur l'état de sécurité du système d'information. 

Bien que nous ayons un serveur Rsyslog centralisant l'ensemble des logs de l'infrastructure, nous installons tout de même l'agent Wazuh sur nos différentes instances et il y a plusieurs raisons à cela :
- La première est que Wazuh transforme les logs, ainsi en ayant à la fois Wazuh et le serveur Rsyslog, il nous est possible d'obtenir un SIEM fonctionnel tout en conservant un format brut des logs de manière centralisé
- La seconde est que l'agent Wazuh ne se contente pas de remonter les logs, il analyse aussi l'intégrité des fichiers ou encore les vulnérabilités, installé l'agent Wazuh uniquement sur le serveur Rsyslog nous empêcherait de profiter de ces fonctionnalités sur le reste de notre infrastructure.

Nous avons configuré Wazuh de telle sorte à ce qu'il nous remonte les alertes à la fois par mail et par Discord permettant d'être informé en temps réel si jamais une tentative d'intrusion au sein de l'infrastructure venait à se produire.

{screen - config alerte discord}
{screen - config alerte mail}

L'installation des différents services de Wazuh ainsi que le déploiement des agents sur les différentes instances est entièrement automatisé, permettant de facilement redéployé le serveur Wazuh en cas de problème ou encore d'ajouter de nouvelles instances au sein du champ de contrôle de notre SIEM.

Voici comment se découpe le playbook deploy_siem charger de l'installation et de la configuration automatisé de Wazuh (l'instance LXC est préalablement déployé grâce au playbook deploy_lxc.yml) :

| Rôle                         | Action                                                                                                      |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------- |
| common/set_certificate_facts | Initialisation des variables pour l'automatisation des tâches liés aux certificats                          |
| common/request_certificate   | Création d'un certificat à partir d'un CSR                                                                  |
| common/set_certificate_facts | Initialisation des variables pour l'automatisation des tâches liés aux certificats (certificat admin Wazuh) |
| common/request_certificate   | Création d'un certificat à partir d'un CSR (certificat admin Wazuh)                                         |
| siem/add_repo                | Ajoute le dépôt Wazuh à la machine                                                                          |
| siem/indexer                 | Installe et configure le service Wazuh indexer                                                              |
| siem/server                  | Installe et configure le service Wazuh server                                                               |
| siem/dashboard               | Installe et configure le service Wazuh Dashboard                                                            |

### Exploitation

#### Ports en écoute
Etant donné que nous avons réalisé une installation All-in-One de Wazuh, un certain nombre de port sont en écoute pour assurer le bon fonctionnement de chaque service. En voici la liste et le détail de chacun de ses ports :
- 22 : Accès SSH 
- 443 : Accès HTTPS; Interface Web de Wazuh fournit par Wazuh Dashboard
- 1514 : Communication avec les agents Wazuh
- 1515 : Enrôlement des agents
- 9200 : API de l'indexer permet au Dashboard et à Filebeat 
- 9300 : Communication interne du cluster OpenSearch
- 55000 : API REST de Wazuh notamment pour que le Dashboard puisse interagir avec le Wazuh Manager 

#### Accès

Le première accès disponible est l'accès SSH rendu disponible à travers le bastion JumpServer (fty-lbst01.cenexis.lan), celui-ci sera principalement utilisé pour s'occuper de la gestion de l'instance. L'accès SSH permettre de réaliser les mis à jour systèmes ou encore de débuguer les différents services, s'ils venaient à être hors-service. L'authentification utilise des clé SSH et est entièrement géré par le bastion.
Le second accès de cette machine se fait via l'interface web à laquelle on accèdera encore une fois à travers le bastion JumpServer. Cet accès servira surtout a exploité le service Wazuh, donc surveillance des différentes instances, analyse des données récupérées ou encore génération de rapport. L'authentification est entièrement managé par le bastion évitant d'avoir à retenir des identifiants d'accès supplémentaires.

#### Gestion général
La machine fty-lsie01.cenexis.lan doit faire opérer 4 services distinct pour que Wazuh fonctionne :

| Service        | Dossier de configuration | Chemin vers logs       |
| -------------- | ------------------------ | ---------------------- |
| wazuh-dahboard | /etc/wazuh-dashboard     | N/A                    |
| wazuh-indexer  | /etc/wazuh-indexer       | /var/log/wazuh-indexer |
| wazuh-manager  | /var/ossec/etc           | /var/ossec/logs        |
| filebeat       | /etc/filebeat            | /var/log/filebeat      |

Pour gérer ces services on utilisera les commandes suivantes :
```bash
systemctl status <nom_du_service> # Pour obtenir rapidement l'état du service
systemctl stop <nom_du_service> # Pour arrêter un service
systemctl start <nom_du_service> # Pour démarrer un serivce
systemctl restart <nom_du_service> # Pour redémarrer un service en FAILED
journalctl -xeu <nom_du_service> # Pour voir les derniers logs du service
```

#### Mis à jour
Avant toute mise à jour, une backup doit être réalisé pour permettre un rollback en cas d'incident suite à l'intervention.
Pour la mis à jour de Wazuh, il suffit d'exécuter la commande suivante :
```bash
sudo dnf update -y wazuh-indexer wazuh-manager wazuh-dashboard filebeat
```

#### Incidents général