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
Il y a en plus de cela besoin d'une base de données de type SQL comme MariaDB ou PostgreSQL, celle-ci peut être externalisé. La solution utilise aussi une base de donnée Redis (clé/valeur), qui peut elle aussi être externalisé. 
Dans notre cas, seul la base de données PostgreSQL a été externalisé.

Pour les accès web tel que Proxmox ou Wazuh, il est obligatoire d'installer une VM Windows supplémentaire qui servira de point relais pour accéder aux interfaces web.

===SOIT===
c'est d'ailleurs ce que nous avons fait notamment, ainsi JumpServer utilise notre cluster de base de données à travers notre HAProxy, pour garantir la haute disponibilité de l'accès à la base de données

===SOIT===
cependant, dans notre cas nous avons fait le choix de la conteneurisé avec les autres services pour faciliter le déploiement et la gestion du service.

Concernant l'installation, celle-ci est quasiment entièrement automatisé, je dis quasiment car il y a une étape qui ne peut malheureusement pas être automatisé et c'est la modification du mot de passe du compte admin.
Cependant le provisioning des terminaux, des comptes, des utilisateurs et des permissions est entièrement automatisé, permettant de facilement ajouté de nouveaux éléments sans avoir à intervenir directement sur l'interface de JumpServer.

