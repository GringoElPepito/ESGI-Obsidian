Un serveur de logs permet de centraliser les journaux d'événements provenant de l'ensemble des équipements d'une infrastructure (serveurs, équipements réseau, applications, etc.). Cette centralisation facilite la supervision, le diagnostic des incidents, les investigations de sécurité et la conservation des traces d'activité. Il permet de préserver une version non-transformée des journaux d'évènements permettant de suivre l'intégralité du déroulement des activités.

Voici les caractéristiques de l'instance **fty-llog01.cenexis.lan** :

|Paramètres|Valeur|
|---|---|
|Type d'instance|LXC|
|OS|Rocky Linux 10.2|
|CPU|1 vCPU|
|RAM|512 Mo|
|Stockage|32 Go|
|Interface|eth0 : VLAN161 (Syslog & SIEM) → 10.16.1.10|

{screen - Summary fty-llog01.cenexis.lan}

## Installation

L'instance **fty-llog01.cenexis.lan** héberge le service **Rsyslog** configuré en mode **serveur** afin de centraliser les journaux provenant de l'ensemble des équipements de l'infrastructure. Cette centralisation permet de conserver une copie brute des événements système générés par les différents serveurs, équipements réseau et services applicatifs, facilitant ainsi les opérations de supervision, d'investigation et d'audit.

Les communications entre les différents clients Syslog et le serveur sont réalisées à l'aide du protocole **RELP (Reliable Event Logging Protocol)**. Contrairement au protocole Syslog classique basé sur UDP ou TCP, RELP garantit la livraison des messages grâce à un mécanisme d'accusé de réception, évitant ainsi toute perte de journaux lors d'une interruption réseau ou d'un redémarrage d'un équipement.

Afin d'assurer la confidentialité et l'intégrité des journaux échangés, les communications RELP sont protégées par **TLS 1.3** avec des mécanismes de chiffrement **Post-Quantiques (PQC)**. Cette configuration permet de renforcer la sécurité des échanges tout en anticipant les futures évolutions des capacités de calcul liées à l'informatique quantique.

Le serveur Rsyslog a pour but de préserver une version brut des logs qui lui sont transmises. Nous avons donc décorrélé le fonctionnement du Rsyslog et du SIEM pour éviter que celles-ci soient altérées.

Concernant la réception et le rangement des logs, l'ensemble des logs est stocké dans le dossier /var/log/remote qui contient pour chaque hôte un dossier spécifique 

L'installation et la configuration du serveur sont entièrement automatisées à l'aide d'Ansible, permettant un redéploiement rapide en cas d'incident ou la reconstruction de l'infrastructure à l'identique.

L'installation et la configuration de Rsyslog est entièrement comprise dans le playbook deploy_lxc.yml permettant le déploiement des instances LXC.
## Exploitation

### Ports en écoute

Le serveur Rsyslog expose uniquement les services nécessaires à son fonctionnement :

- **22** : Accès SSH (uniquement via le serveur de bastion)
- **20514** : Réception des journaux Syslog via le protocole **RELP** sécurisé par **TLS 1.3**

### Accès

L'administration de l'instance est exclusivement réalisée via un accès **SSH** transitant par le serveur de bastion de l'infrastructure. Aucune connexion SSH directe depuis le réseau utilisateur n'est autorisée.

L'authentification repose exclusivement sur des clés SSH gérées par le bastion, garantissant un contrôle centralisé des accès administrateurs.

Le service Rsyslog ne dispose d'aucune interface Web. Son administration s'effectue uniquement depuis la ligne de commande.

## Gestion générale

Le fonctionnement de cette machine repose principalement sur le service **rsyslog**.

|Service|Dossier de configuration|Chemin des journaux|
|---|---|---|
|rsyslog|/etc/rsyslog.conf et /etc/rsyslog.d/|/var/log|

Les principales commandes d'administration sont les suivantes :

```bash
systemctl status rsyslog      # Vérifier l'état du service
systemctl stop rsyslog        # Arrêter le service
systemctl start rsyslog       # Démarrer le service
systemctl restart rsyslog     # Redémarrer le service
journalctl -xeu rsyslog       # Consulter les journaux du service
```

## Mise à jour

Avant toute intervention, une sauvegarde de la machine doit être réalisée afin de permettre un retour arrière en cas d'incident.

La mise à jour du système ainsi que du service Rsyslog s'effectue simplement à l'aide de la commande suivante :

```bash
sudo dnf update -y
```

Une fois la mise à jour terminée, il est recommandé de redémarrer le service Rsyslog afin de prendre en compte les éventuelles nouvelles versions des composants installés :

```bash
sudo systemctl restart rsyslog
```