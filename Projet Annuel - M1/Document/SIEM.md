La mise en place d'une solution de gestion des informations et des événements de sécurité (SIEM) est essentielle pour une entreprise exerçant une activité critique et traitant des données sensibles. En centralisant et en analysant en temps réel les journaux d'événements issus des différents équipements et applications, le SIEM permet de détecter rapidement les comportements anormaux, les tentatives d'intrusion et les incidents de sécurité. Il améliore la capacité de réaction des équipes, facilite les investigations grâce à la conservation des traces d'activité et contribue au respect des exigences réglementaires en matière de sécurité et de traçabilité. En offrant une visibilité globale sur l'état de sécurité du système d'information, le SIEM renforce la résilience de l'entreprise face aux cybermenaces.

Voici les caractéristiques de l'instance siem.cenexis.lan :
- CPU : 2 cores
- RAM :  4G
- Stockage : 64G
- Instance : LXC
- OS : Rocky Linux 10.2
- état HA attendu : started
- Réseau
	- VLAN : VLAN 161 - Syslog & SIEM
	- IP : 10.16.1.20
{screen - Summary siem.cenexis.lan}

Dans notre cas, nous avons optés pour la solution Wazuh qui est une solution SIEM Open-Source ayant à de nombreuses reprises fait ses preuves en proposant un système d'analyse complet allant au-delà de la simple analyse de logs. Notre installation de Wazuh est une installation All-in-One,  c'est à dire que l'ensemble des composants de la solution est installé sur un même serveur. Cette configuration regroupe le **Wazuh Server**, chargé de recevoir et d'analyser les événements de sécurité transmis par les agents, le **Wazuh Indexer**, qui stocke et indexe les données pour permettre leur recherche et leur corrélation, ainsi que le **Wazuh Dashboard**, qui fournit une interface web de supervision, de visualisation et d'administration. 
Les agents Wazuh, déployés sur les postes de travail, les serveurs et autres équipements compatibles, collectent les journaux système, surveillent l'intégrité des fichiers, détectent les vulnérabilités, contrôlent la conformité des configurations et remontent les événements de sécurité vers le serveur. Les règles de détection intégrées permettent d'identifier les comportements suspects et de générer des alertes en temps réel. Cette architecture centralisée simplifie le déploiement, l'administration et la maintenance de la plateforme, tout en offrant une visibilité complète sur l'état de sécurité du système d'information. 

Bien que nous ayons un serveur Rsyslog centralisant l'ensemble des logs de l'infrastructure, nous installons tout de même l'agent Wazuh sur nos différentes instances et il y a plusieurs raisons à cela :
- La première est que Wazuh transforme les logs, ainsi en ayant à la fois Wazuh et le serveur Rsyslog, il nous est possible d'obtenir un SIEM fonctionnel tout en conservant un format brut des logs de manière centralisé
- La seconde est que l'agent Wazuh ne se contente pas de remonter les logs, il analyse aussi l'intégrité des fichiers ou encore les vulnérabilités, installé l'agent Wazuh uniquement sur le serveur Rsyslog nous empêcherait de profiter de ces fonctionnalités sur le reste de notre infrastructure.

L'installation des différents services de Wazuh ainsi que le déploiement des agents sur les différentes instances est entièrement automatisé, permettant de facilement redéployé le serveur Wazuh en cas de problème ou encore d'ajouter de nouvelles instances au sein du champ de contrôle de notre SIEM.