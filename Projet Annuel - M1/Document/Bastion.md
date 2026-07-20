Dans toutes infrastructures informatiques, il est important de contrôler l'accès au serveur, pour garantir qu'uniquement les personnes autorisées puissent s'y connecter. C'est pour répondre à ce besoin que les solutions de bastions ont commencées à émerger. Avec un bastion, concentre tous les accès aux services et machines sensibles à un seul et unique endroit de l'infrastructure.
La mise en place d'un bastion d'administration, constitue une mesure essentielle pour sécuriser les accès aux systèmes d'une infrastructure informatique critique. En centralisant les connexions des administrateurs vers les serveurs, équipements réseau et autres ressources sensibles, le bastion supprime les accès directs et réduit significativement la surface d'attaque. Il permet de mettre en œuvre une authentification forte, de contrôler les autorisations selon le principe du moindre privilège, d'enregistrer les sessions d'administration et de conserver une traçabilité complète des actions réalisées. Cette centralisation facilite les audits de sécurité, la détection des comportements anormaux et les investigations en cas d'incident.

Cette approche s'inscrit pleinement dans les recommandations de l'ANSSI, qui préconise de cloisonner les environnements d'administration, de limiter les accès privilégiés aux seules personnes habilitées et de renforcer la supervision des actions réalisées sur les systèmes critiques. Les guides de l'ANSSI relatifs à l'administration sécurisée et à l'hygiène informatique insistent notamment sur l'utilisation d'un point d'accès dédié pour les opérations d'administration, associé à une authentification multifacteur, à une journalisation exhaustive et à une gestion rigoureuse des comptes à privilèges. En répondant à ces bonnes pratiques, un bastion contribue à améliorer la résilience du système d'information face aux cyberattaques, à limiter les risques de compromission des comptes administrateurs et à satisfaire les exigences de sécurité applicables aux organismes traitant des données sensibles ou exerçant une activité critique.

Voici les caractéristiques de l'instance fty-lbst01.cenexis.lan :

| Paramètres      | Valeur                      |
| --------------- | --------------------------- |
| Type d'instance | VM                          |
| OS              | Rocky 10.2                  |
| CPU             | 4 vCPU                      |
| RAM             | 4G                          |
| Stockage        | 30G                         |
| Interfaces      | eth0: VLAN103 -> 10.10.3.10 |
{screen - Summary fty-lbst01.cenexis.lan}

Comme expliqué précédemment dans la section des choix technologiques, nous avons optés pour la solution JumpServer. JumpServer est une solution conteneurisée pouvant fonctionné sur Docker ou des orchestrateurs comme Kubernetes. Elle se découpe en plusieurs micro-services :
- `Core` : le service au centre de JumpServer, concentrant l'API et toute la logique permettant de faire fonctionner ce système.
- `Lina` : Ce composant est l'interface web permettant d'administrer JumpServer.
- `Luna` : Ce composant est l'interface Web fournissant l'accès aux différents terminaux enregistrés sur JumpServer.
- `KoKo` : Ce composant se charge de la gestion des connexions CLI (SSH, Telnet, Kubernetes etc..) avec les terminaux concernés
- `Lion` : Ce composant se charge de la gestion des connexions graphique (RDP, HTTP, HTTPS)
- `Chen` : Base de données interne à JumpServer pour la gestion de l'interface Web
Il y a en plus de cela besoin d'une base de données de type SQL comme MariaDB ou PostgreSQL, celle-ci peut être externalisé. La solution utilise aussi une base de donnée Redis (clé/valeur), qui peut elle aussi être externalisé. Enfin JumpServer nécessite un dernier service, Celery qui est une solution d'orchestration et de gestion de tâche, celui-ci ne peut pas être externalisé et doit donc forcément être conteneurisé.
Dans notre cas, seul la base de données PostgreSQL a été externalisé, ainsi JumpServer peut profiter de notre cluster de base de données PostgreSQL. Cependant JumpServer étant le seul service utilisant Redis, nous avons décidé de le laisser conteneurisé.

Pour les accès web tel que Proxmox ou Wazuh, il est obligatoire d'installer une VM Windows supplémentaire qui servira de point relais pour accéder aux interfaces web.


Concernant l'installation, celle-ci est quasiment entièrement automatisé, je dis quasiment car il y a une étape qui ne peut malheureusement pas être automatisé et c'est la modification du mot de passe du compte admin.
Cependant le provisioning des terminaux, des comptes, des utilisateurs et des permissions est entièrement automatisé, permettant de facilement ajouté de nouveaux éléments sans avoir à intervenir directement sur l'interface de JumpServer.

Voici comment se découpe le playbook deploy_jumpser.yml charger de l'installation et de la configuration automatisé de JumpServer (l'instance VM est déployé manuellement) :

| Rôle                               | Action                                                                                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------------- |
| common/set_certificate_facts       | Initialisation des variables pour l'automatisation des tâches liés aux certificats                |
| common/request_certificate         | Création d'un certificat à partir d'un CSR                                                        |
| jumpserver/install_jumpserver      | Installe et réalise la configuration de base de JumpServer                                        |
| jumpserver/init_requests           | Prépare les informations nécessaire à la configuration de JumpServer via l'API                    |
| jumpserver/create_assets           | Créé les serveurs dans jumpserver en se basant sur l'inventaire ansible                           |
| jumpserver/create_accounts         | Crée les comptes de connexions des serveurs dans jumpserver en se basant sur l'inventaire ansible |
| jumpserver/init_requests           | Prépare les informations nécessaire à la configuration de JumpServer via l'API                    |
| jumpserver/create_groups           | Créé les groupes dans jumpserver                                                                  |
| jumpserver/create_users            | Créé les utilisateurs dans jumpserver                                                             |
| jumpserver/create_nodes            | Créé les dossiers pour trier les serveurs dans jumpserver                                         |
| jumpserver/create_unmanaged_assets | Créé les serveurs n'appartenant pas à l'inventaire ansible dans jumpserver                        |
| jumpserver/create_accounts         | Créé les comptes des serveur n'appartenant pas à l'inventaire ansible dans jumpserver             |
| jumpserver/create_permissions      | Créé les permissions pour donner les droits de connexions aux utilisateurs                        |
### Exploitation

#### Ports en écoute
JumpServer se découpant en plusieurs micro-services, un certain nombre de port sont en écoute pour assurer le bon fonctionnement de chaque service. En voici la liste et le détail de chacun de ses ports :
- 22 : Accès SSH 
- 80 : Accès HTTP, ne sert qu'à la direction vers HTTPS
- 443 : Accès HTTPS, WebUI JumpServer
- 323 : NTP
- 2222 : Accès SSH d'utilisation JumpServer (équivalent WebUI en CLI)

#### Accès

Le première accès disponible est l'accès SSH sur le port 22 qui est directement disponible depuis le VLAN 104 (IT Services). Cet accès fournit directement la main sur la machine et servira donc à la gestion de l'instance en elle-même. Cet accès SSH permet de réaliser les mis à jour systèmes ou encore de débuguer les différents services, s'ils venaient à être hors-service. L'authentification utilise des clé SSH et est entièrement géré par le bastion.

Le second accès disponible est l'accès Web qui sert principalement à gérer et utiliser la solution JumpServer elle-même. Via cette interface Web, il est possible de se connecter aux différentes instances préalablement ajoutés, ou encore de gérer les différents éléments (Serveurs, Utilisateurs, Groupes, Comptes de connexion).

Le troisième et dernier accès est l'accès SSH sur le port 2222 qui est lui aussi directement disponible depuis le VLAN104(IT Services). Cependant celui-ci ne donne pas un accès direct à la machine, il donne lieu à une application CLI proposant un service similaire à ce qu'on pourrait trouver sur l'interface Web. En effet, depuis cet accès, il est possible de se connecter aux différents serveurs (via leur accès CLI uniquement).

#### Gestion général
Tous les services propres à JumpServer, mise à part la base de données SQL, sont conteneurisé via Docker.

Pour gérer ces services on utilisera les commandes suivantes :
```bash
docker compose ps # État des conteneurs 
docker compose logs -f postfix-mailcow # Journaux du service SMTP 
docker compose logs -f dovecot-mailcow # Journaux du service IMAP/POP3
docker compose logs -f nginx-mailcow # Journaux du proxy HTTPS 
docker compose logs -f rspamd-mailcow # Journaux du moteur anti-spam 
docker compose restart postfix-mailcow # Redémarrage d'un service 
docker compose down # Arrêt de la plateforme
```