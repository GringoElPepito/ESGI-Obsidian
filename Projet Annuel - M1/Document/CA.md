Pour une entreprise traitant des données sensibles, disposer d'une autorité de certification (CA) locale est un élément essentiel de la sécurité de son infrastructure informatique. Une CA interne permet de générer et de gérer des certificats numériques sans dépendre d'un tiers, garantissant ainsi un contrôle total sur les identités, les communications chiffrées et les accès aux ressources de l'entreprise. Cette maîtrise réduit les risques liés à la compromission des échanges, facilite l'authentification des utilisateurs et des équipements, et contribue au respect des exigences de conformité et de confidentialité. En centralisant la gestion des certificats, l'entreprise renforce également sa capacité à révoquer rapidement des accès en cas d'incident et à maintenir un niveau de sécurité élevé sur l'ensemble de son système d'information.

Pour garantir un certain niveau de sécurité, nous avons opté pour une architecture par délégation. En effet nous avons mis en place une Autorité de certification racine ainsi qu'une Autorité de certification délégué. L'autorité racine émet un certificat de délégation qui permet à l'autorité délégué d'émettre des certificats soumis à certaines contraintes permettant de limiter l'impact potentiel d'une compromission de l'autorité délégué. 
De cette manière, il nous est possible d'éteindre l'autorité racine dans le but de rendre celle-ci inaccessible et limiter le risque de compromission de la clé privé. Pour nous assuré que l'autorité racine reste bien éteinte, une supervision remonte une alerte lorsque l'instance répond au ping.

Voici les caractéristiques de l'instance root-ca.cenexis.lan :

| Paramètres      | Valeur                      |
| --------------- | --------------------------- |
| Type d'instance | LXC                         |
| OS              | Rocky 10.2                  |
| CPU             | 1 vCPU                      |
| RAM             | 512Mb                       |
| Stockage        | 16G                         |
| Interfaces      | eth0: VLAN112 -> 10.11.2.20 |
{screen - Summary root-ca.cenexis.lan}

Voici les caractéristiques de l'instance sub-ca.cenexis.lan :

| Paramètres      | Valeur                      |
| --------------- | --------------------------- |
| Type d'instance | LXC                         |
| OS              | Rocky 10.2                  |
| CPU             | 1 vCPU                      |
| RAM             | 512Mb                       |
| Stockage        | 16G                         |
| Interfaces      | eth0: VLAN112 -> 10.11.2.21 |
{screen - Summary sub-ca.cenexis.lan}

Le déploiement et la configuration des autorités de certification racine et délégué sont entièrement automatisé via des playbook Ansible tout comme la génération de certificat client et serveur permettant de faciliter la configuration de nos différents services.

Pour la création de l'autorité de certification, nous avons utilisé la solution OpenSSL qui sert de base à la plupart des autres solutions existantes sur Linux pour réaliser une autorité de certification. Pour gérer au mieux l'autorité de certification et garantir une cohérence dans la chaîne de certification, nous utilisons un fichier de configuration dédié et généré automatiquement au déploiement des instances.

Une fois généré, les certificats racine et délégué sont par la suite récupérer par l'instance Ansible qui va ensuite se charger d'ajouter la chaîne de certification sur toutes les instances déjà déployés et toutes celles qui le seront déployés par la suite.

Nous avons mis en place 2 chaînes de certification distinctes, la première est une chaîne de certification classique, cependant la seconde est une chaîne de certification Post-Quantique. Cette seconde chaîne nous permet de préparer notre infrastructure aux futures attaques qui seront rendus possible avec la démocratisation de l'informatique quantique. Malheureusement à ce jour, la plupart des services et protocoles ne supportent pas encore les algorithmes liés au chiffrement Post-Quantique, c'est la raison pour laquelle nous maintenons 2 chaînes de certification. De cette manière nous pouvons fournir un chiffrement adapté à tous nos services et faire la migration vers le chiffrement Post-Quantique si les mis à jours de nos services ajoutent le support de celui-ci.

Voici comment se découpe le playbook deploy_ca.yml charger de l'installation et de la configuration automatisé de Wazuh (l'instance LXC est préalablement déployé grâce au playbook deploy_lxc.yml) :

| Rôle                 | Action                                                                                                                 |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| ca/ca_directories    | Créer l'arborescence et les fichiers de configuration pour l'autorité de certification Post-Quantique                  |
| ca/create_ca_cmd     | Créer l'autorité de certification Post-Quantique avec la génération des différents certificats Racine et Intermédiaire |
| ca/ca_directories    | Créer l'arborescence et les fichiers de configuration pour l'autorité de certification classique                       |
| ca/create_ca_cmd     | Créer l'autorité de certification classique avec la génération des différents certificats Racine et Intermédiaire      |
| common/stop_instance | Eteint l'autorité de certification Racine                                                                              |
### Exploitation

#### Ports en écoute
Etant donné que nous avons réalisé une installation All-in-One de Wazuh, un certain nombre de port sont en écoute pour assurer le bon fonctionnement de chaque service. En voici la liste et le détail de chacun de ses ports :
- 22 : Accès SSH 

#### Accès

Le première accès disponible est l'accès SSH rendu disponible à travers le bastion JumpServer (fty-lbst01.cenexis.lan), celui-ci sera principalement utilisé pour s'occuper de la gestion de l'instance. L'accès SSH permettre de réaliser les mis à jour systèmes ou encore de débuguer les différents services, s'ils venaient à être hors-service. L'authentification utilise des clé SSH et est entièrement géré par le bastion.

#### Gestion général
Il n'y a pas de service fonctionnant en permanence pour l'autorité de certification, tout se passe avec des commandes ad-hoc réalisé en fonction des besoins.

| Hôte    | Autorité                     | Emplacement               |
| ------- | ---------------------------- | ------------------------- |
| root-ca | Racine                       | /home/desigual/root-ca    |
| root-ca | Racine Post-Quantique        | /home/desigual/root-ca-pq |
| sub-ca  | Intermédiaire                | /home/desigual/sub-ca     |
| sub-ca  | Intermédiaire Post-Quantique | /home/desigual/sub-ca-pq  |
L'arborescence de chacun de ces dossiers se découpe de la manière suivante :

| Fichier/Dossier | Rôle                                                              |
| --------------- | ----------------------------------------------------------------- |
| certs           | Contient les certificats générés                                  |
| db              | Contient les informations liés à la gestion des certificats       |
| private         | Contient les clés privés liés au certificats                      |
| req             | Contient les demandes de création de certificat                   |
| *.conf          | Fichier contenant la configuration de l'autorité de certification |
Pour gérer le remplacement de certificat, il suffit de relancer les playbooks liés au déploiement de chaque service, pour que ces derniers soient mis à jour automatiquement.
#### Mise à jour
Avant toute mise à jour, une backup doit être réalisé pour permettre un rollback en cas d'incident suite à l'intervention (cf. Procédure de sauvegarde).
Pour la mis à jour de OpenSSL, il suffit d'exécuter la commande suivante :
```bash
sudo dnf update -y
```