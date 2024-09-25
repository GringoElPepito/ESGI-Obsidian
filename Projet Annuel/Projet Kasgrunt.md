Hyperviseur : ESXi
2 bureau distant de 150 km
Mis à jour du système d'info.
infra attendu :
Site PRA, Site principal, Site secondaire

Infra réseau/sécurité :
- iaisons VPN IPSEC entre les différents sites
- Switching en VLAN
- Deux niveaux de firewalls
- Redondance de firewalls
- Routage avec les firewalls si possible, sinon avec routeurs Cisco

Infra virtu :
- Serveurs sous VMWare ESXi
- Gestion des serveurs avec vCenter
- VSphere Replication
- Les VM doivent être hautement disponibles (en cas de panne sur un serveur, un autre doit prendre le relai)
- Migration à chaud des VM (automatique pour les élèves avancés (selon les charges CPU, ou RAM ou espace disque))

Infra sauvegarde :
- Sauvegarde des VM avec une solution dédié ou recommandé pour les serveurs ESXi
- Sauvegarde des fichiers
- Sauvegardes incrémentielles
- Sauvegardes de fichier façont time machine

Infra stokage :
- Stockage SAN sur une solution de SAN virtualisé
- Mis en cluster du stockage, via une solution hyperconvergée ou du stockage partagé
- Solution de partage de fichier tels que Dropbox ou Google Drive mais déployé en local

Infra system et com :
- Serveur AD ou Azure AD.
- Maxé Linux
- téléphonie IP redondée
- Accès téléphonique
- Messagerie Mattermost Dovecot 

Infra supervision
- Monitoring entrées sorties
- Monitoring services et ordinateurs
- Alerting par mail
- métrologie réseau et serveurs
- serveur syslog et mis en place d'un SIEM 

Faire devis pour matos
Diagram de Gantt
Matrice
SWOT

Document technique :
Volet financier max 10 pages
Volet méthodologie max 20 pages
Volet technique min 40 pages

Analyse de risques, études de tous les risques potentielles liées à l'infrastructure :
- panne électrique
- panne matérielle
- failles de sécurité
- 

Matrice de risque

Powerpoint à présenté de 10min volet financier, méthodo, technique :
20min pour présentation de 4 à 6 scénarios.
Création de panne ça peut être un banger.

Orga projet pixel, répartition des tâches systèmes de rapport, trello



