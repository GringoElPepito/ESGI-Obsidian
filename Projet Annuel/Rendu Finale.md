# Introduction

# Finance

## Virtualisation
6 serveurs de virtualisation Dell PowerEdge R6615 Serveur Rack Plus :
- AMD EPYC 9354P 3,25 GHz 32C/64T x 1
- 32 Go RDIMM, 5600 MT/s x 2
- SSD 960Go DATA x 2
## Stockage :

6 serveurs de stockage TrueNAS M-30
# Méthodologie

# Technique

## Stockage
RAIDZ2 type de RAID fonctionnement uniquement avec le système de fichier ZFS
## Réseau
### LAN
Le routage est assuré par 2 routeurs cisco qui font le liens entre les pare-feux interne et le LAN.
2 NETCORE se charge de la centralisation du trafic réseau du LAN en servant d'intermédiaire entre les routeurs et les switch de distribution
Les switch de distribution sont connectés aux 2 NETCORE et fournissent l'accès au réseau aux terminaux (poste utilisateur, imprimantes etc...)

### WAN
2 firewalls externes OPNsense
2 firewalls internes Pfsense

### DMZ



# Doc vivien
1. Introduction
Dans un contexte professionnel, la supervision des systèmes d'information est essentielle pour garantir la disponibilité, la performance et la sécurité des services. Ce projet vise à mettre en œuvre une solution de supervision basée sur Zabbix, permettant de surveiller des machines Windows et Linux, ainsi que des services critiques, tout en assurant une gestion fine des alertes et des utilisateurs.
2. Analyse des besoins
Le cahier des charges impose la supervision de plusieurs types de machines : un serveur Windows Active Directory, un poste Windows 10, et un serveur Debian hébergeant des services tels qu'Apache2, FTP, SSH, MariaDB et WordPress. Les besoins incluent également la mise en place de déclencheurs personnalisés, de notifications par mail, et d'une interface graphique adaptée à la visualisation rapide des métriques critiques.
3. Architecture technique
Tous les composants de la solution Zabbix ont été installés sur un seul serveur Rocky Linux, incluant le serveur Zabbix, le frontend web et la base de données MariaDB. Cette architecture simplifiée permet un déploiement rapide et une gestion centralisée. Les agents Zabbix sont déployés sur les machines clientes, configurés en mode actif pour remonter les métriques vers le serveur.
4. Mise en œuvre technique
La supervision repose sur des agents Zabbix actifs, configurés pour remonter automatiquement les métriques système. Un template personnalisé a été créé pour superviser les machines Linux, incluant des items pour l’usage CPU, RAM, disque, et les services critiques. Des déclencheurs ont été définis pour alerter les administrateurs en cas de surcharge CPU ou RAM (au-delà de 95% pendant plus de 5 minutes). Le dashboard Zabbix a été modifié pour afficher en priorité des graphiques représentant l’usage CPU et RAM, facilitant ainsi la prise de décision rapide.
# Optionnel si on le fait vraiment 5. Sécurité
La sécurité a été assurée par la configuration du service SSH, la restriction des ports via iptables, et la gestion des droits d’accès dans l’interface Zabbix. Les agents sont configurés pour n’accepter que les connexions du serveur de supervision, et les utilisateurs Zabbix sont segmentés par rôles (Linux, Windows).
# Optionnel si on fait aussi les tests 6. Tests et validation
Des tests ont été réalisés pour valider le bon fonctionnement de la supervision : surcharge CPU/RAM simulée, arrêt de services critiques, et vérification des alertes par mail. Les graphiques du dashboard ont été vérifiés pour refléter correctement l’état des machines supervisées.
7. Résultats obtenus
Le système de supervision permet une visualisation centralisée, une détection proactive des anomalies, et une notification ciblée des administrateurs. L’interface personnalisée améliore la réactivité face aux incidents.
8. Difficultés rencontrées
Des difficultés ont été rencontrées lors de la configuration des agents Windows, la création de déclencheurs complexes, et la gestion fine des droits utilisateurs. Ces problèmes ont été résolus grâce à la documentation officielle et des tests itératifs.
9. Conclusion
Ce projet a permis de mettre en place une solution de supervision complète et fiable. Il a été l’occasion de développer des compétences en administration système, supervision, sécurité et automatisation. Des améliorations futures pourraient inclure l’intégration de Grafana ou la mise en place d’un cluster Zabbix haute disponibilité.
10. Annexes
Scripts de redémarrage, fichiers de configuration, captures d’écran, templates Zabbix exportés, etc.
11. Bibliographie
- Documentation officielle Zabbix
- Forums communautaires Zabbix
- Tutoriels techniques (Linux, supervision, sécurité)
- Cours et supports pédagogiques de l’ESGI