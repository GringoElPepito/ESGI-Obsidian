Supervision : 
- Définition : consiste à surveiller en temps réel le bon foncitonnement des systèmes et à réagir rapidement en cas de problème. Elle se base souvent sur des alertes et des notifications automatiques.
- Outils : Zabbix, Centreon, Nagios
Métrologie : 
- Définition : récupération des valeurs de métrique.

Le corum nombre de panne possible pour un cluster, nombre de node / 2 + 1

# Architecture de Supervision
Supervision centralisée : Tous les systèmes rapportent à un serveur central
Supervision distribuée : Plusieurs serveurs de supervision pour des sites ou des services spécifiques, clusterisation.
Hiérarchisation : Prioriser les services critiques pour l'entreprise.

Services Zabbix :
- Serveur
- Base de données
- Frontend
- Proxy

Services Nagios :
- Serveur
- Pollers

Segmentation des services

# SNMP
collecte d'informations sur les équipements réseau (switches, routeurs, serveurs)
Fonctionnement
UDP 161 serveur
UDP 162 client

## MIB
sorte de base de données, identifiant chaque élément de la machine supervisé