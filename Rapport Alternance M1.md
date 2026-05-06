# Remerciements

  

Avant toute chose, j'aimerais adresser mes plus sincères remerciements aux membres de l'équipe ingénierie Socle Réseau de SFR. Je me suis senti assez vite intégré à l'équipe grâce à la convivialité de chacun de ses membres.

Plus particulièrement je souhaite remercier Benoît PIETRI qui a plusieurs fois pris de son temps pour m'expliquer certains concepts ou encore m'aider à investiguer sur les points bloquants que je pouvais rencontrer au cours de mes missions.

Enfin, je souhaiterais remercier Marc YALAP, mon tuteur, sans qui cette alternance n'aurait pu être possible et qui lui aussi a pris le temps de m'accompagner au sein de ma prise de poste chez SFR.

# Sommaire

  

- Introduction

- Partie 1 : Contexte de l'entreprise

- Partie 2 : Missions réalisés

- Partie 3 : Bilan et recul sur les missions

- Conclusion

- Annexes

# Introduction

Ce rapport à pour sujet mon alternance au sein de SFR, le but est de d'aborder mon intégration au sein de l'équipe ingénierie Socle Réseau ainsi que les différentes missions que j'ai pu réalisé en tant que membre de cette équipe tout en portant un œil critique sur le déroulement de cette alternance. 

Mon alternance a débuté le 8 septembre 2025, j'ai à cette occasion rejoint l'équipe ingénierie Socle Réseau

Dans un premier temps, je détaillerai l'entreprise de son ensemble, en abordant notamment son statut juridique, son chiffre d'affaire ou encore son secteur d'activité. Par la suite, je me concentrerai plus spécifiquement sur le service au sein du quel j'ai eu la chance d'évoluer au cours de cette alternance, en détaillant les différentes missions relatives à l'équipe ingénierie Socle Réseau ainsi que les membres de cette dernière.

Suite à cela, je me concentrerais sur les différentes missions que j'ai pu réaliser depuis le début de cette alternance. Enfin je réaliserai un bilan de cette alternance ainsi que des missions réalisées.

# Entreprise

  

## Présentation de l'entreprise

  

## Présentation du service

  

Mon alternance se déroule au sein de l'équipe ingénierie Socle Réseau appartenant au service Réseau sous la direction d'Eric JAN.

Le but de cette équipe est la mise en place et la maintenance de différents environnements informatique permettant le déploiement d'application et de système géré par d'autres équipes au sein de SFR. Tous ces environnements sont nommés socles et peuvent être comparés à des clouds privés, l'objectif étant de mettre à disposition un ensemble de ressource (calcul, stockage, réseau etc...). Notre équipe réalise la mise en place de l'infrastructure de bout en bout ce qui comprend l'étude des solutions techniques, la budgétisation, la conception de l'infrastructure, la gestion du matérielle et des licences, la mise en place de l'infrastructure et des process liés, l'automatisation des opérations récurrentes ainsi que le maintien et l'évolution de l'infrastructure.

De plus pour chaque socle, notre équipe se charge de chaque partie de l'infrastructure c'est à dire la gestion du matériel réseau, des serveurs, du stockage, de la sauvegarde ainsi que de la solution logicielle (VMWare, OpenShift, OpenStack, FusionSphere etc...) au centre de chacun des socles.

L'équipe compte une dizaine de personne (cf. Annexe Organigramme).
# Partie 2 : Mission

  

## Fonction occupée et missions
Au cours de cette alternance, j'occupe le poste d'ingénieur développement et socle, voici les missions liés à cette fonction :
- Maintien des automatisations existantes sur les différents environnements en gestion au sein de l'équipe socle
- Création de nouve
- 

  
  

## Automatisation MAJ LUN de Boot

  

### Objectifs :

- Automatiser une tâche critique qui est la mis à jour des configurations des serveurs de calcul

- Documenter le développement et l'utilisation des automatisations produites pour l'exploitation

- Assurer un suivi en corrigeant les erreurs pouvant apparaître au cours de l'exploitation des automatisations.

  

La major partie des serveurs fournissant les ressources de calcul à nos socles sont des serveurs Cisco UCS. Ces machines sont assez particulière car elles ne possèdent pas de stockage et nécessite absolument la mise en place d'un SAN (Storage Area Network) reposant sur des baies de stockage Pure Storage ou NetApp en fonction des socles. Cependant les baies de stockage de nos socles se rapprochent de leur date de fin de support officiel, certaines l'ont par ailleurs même déjà atteinte. Pour maintenir le fonctionnement de nos différents socles, ils nous faut donc remplacer ces baies informatiques ce qui implique la modification des configurations des serveurs UCS.

Pour gérer les différents serveurs UCS et notamment paramétrer leur connexion au SAN, nous utilisons l'UCS Manager qui se présente sous la forme d'une interface web et d'une API.

Les serveurs UCS fournissent la puissance de calcul nécessaire au bon fonctionnement des applications servant à la production de SFR, il faut donc limiter au minimum la durée où ceux-ci sont en maintenance, ce qui requiert de réduire le temps d'intervention ainsi que le risque d'erreur pouvant rallonger ce dernier.

C'est pour cela, que l'on m'a demandé d'automatiser les différentes nécessaires à la modification des configuration des serveurs UCS. Au sein de l'UCS Manager, chaque serveur possède ce qu'on appelle un Service Profile qui est un fichier de configuration. Chacun de ces Service Profiles peut être lié à un Template (Modèle/Patron). De cette manière, le Service Profile récupère tous les paramètres définis au sein du Template ce qui permet d'uniformiser les configurations d'un Serveur à l'autre.

Avant de passer à la réalisation de l'automatisation, il est indispensable de comprendre le process de A à Z, une bonne pratique est de se baser sur une procédure manuelle pour réaliser l'automatisation, de cette façon, il est possible de réduire le delta entre la réalisation manuelle et automatisée.

Pour ce projet, voici les différentes étapes devant être automatisées :

- Création d'un clone des Templates dans le but d'avoir un backup de configuration en cas de problème

- Liaison des Service Profiles vers les Templates clone

- Vérification qu'aucun Service Profile n'est lié aux Templates d'origine puis, modification des Templates d'origine (notamment paramétrage de l'accès au stockage)

- Liaison des Service Profiles vers les Templates d'origine

- Vérification qu'aucun Service Profile n'est lié au Template clone puis, suppression des Templates clone

  
  

### Amélioration du cycle de développement des automatisations

Au cours de son existence, l'équipe ingénierie socle réseau a créé de nombreuses automatisations dans le but de standardiser et faciliter la gestion des différents environnements sur lesquelles elle intervient. Cependant de nombreuses personnes sont intervenus pour la création, l'amélioration ou le maintien de ces automatisations. La base de code n'a cessé de croître sans avoir de direction claire ce qui se retranscrit avec un code non uniforme d'un projet à l'autre ou au sein d'un même projet, des bonnes pratiques pas toujours respectés ou encore des deltas entre les environnements de développement et de production. Tous ces éléments rendent l'exploitation et le maintien des automatisations de plus en plus complexe au fil du temps.

C'est en constatant cela que j'ai entrepris une mission d'amélioration du cycle de développement. Voici les objectifs de cette mission :

- Définition d'un ensemble de règle de développement dans le but d'uniformiser le code pour faciliter son maintien au cours du temps

- Standardisation des commits pour améliorer la lisibilité de ces derniers

- Mis en place d'un processus de développement claire permettant de cadrer le cycle de développement de chaque projet d'automatisation.

- Fournir un environnement de développement standard - Cet objectif étant assez complexe fait l'objet d'une mission à par entière.

  
  

### Création d'un environnement de développement

Dans le cadre de l'amélioration du cycle de développement, le sujet de mettre en place un nouvel environnement de développement est apparu d'autant plus que lors de mon arrivée au sein de l'équipe Ingénierie Socle Réseau celle-ci travaillait sur le projet KORN (Kubernetes Outils Réseau NMS). Le projet KORN vise à déployer un socle basé sur la technologie OpenShift (Surcouche à Kubernetes) de Red Hat. Contrairement aux socles déjà existants mettant en avant la virtualisation avec par exemple VMWare ESXi, ici la technologie se concentre sur la conteneurisation. Ce qui offre de nouvelles possibilités mais implique aussi de nouvelles contraintes, comme l'apprentissage des paradigmes propre à Kubernetes et plus largement à la conteneurisation. C'est dans ce contexte que Benoit PIETRI m'a demandé de réaliser un POC sur la mise en place d'un environnement de développement sur ce nouveau socle. Voici les objectifs de cette mission :

- Mis en place d'un environnement de développement comprenant les fonctionnalités suivantes :

    - un IDE type Visual Studio Code

    - Possibilité de récupérer un projet depuis un dépôt GitLab et de mettre à jour le dépôt

    - Possibilité de construire des images Docker et de les enregistrer sur Quay (Registre d'image Docker créé par Red Hat)

- Documentation de la mise en place et de la gestion de la solution.

- Industrialisation de la solution pour le fournir au client du socle KORN.

-

Dans un premier temps, cette solution servira a fournir à notre équipe des systèmes standard dédiés au développement pour faciliter la création et le maintien de nos automatisations. Si les tests sont concluants et que la technologie le permet alors la solution sera potentiellement proposé en tant que service aux clients du socle KORN.

Kubernetes permet l'installation d'opérateur, qui sont pour faire simple des applications fournissant un service ou étendant les fonctionnalités de Kubernetes. Dans le cas d'OpenShift, Red Hat propose un catalogue d'opérateur développé par Red Hat ou qui sont certifié par Red Hat garantissant une meilleur compatibilité entre l'environnement Kubernetes OpenShift et l'opérateur. Ce catalogue est assez fournit et propose par ailleurs un opérateur semblant répondre à mon besoin, à savoir Red Hat OpenShift Dev Spaces. En effet, cet opérateur a pour but de créer des environnements de développement à partir d'un fichier `.devfile` définissant les caractéristiques de l'environnement à créer.

  

Cette mission m'a permise de travailler div

### Intégration de RHEL 10

Le 20 Mai 2025, Red Hat a sortie la version 10 de Red Hat Enterprise Linux (RHEL). Dans le but de maintenir à jour le catalogue d'OS de notre socle NMS (Socle hébergeant les outils internes à SFR), nous nous devons d'intégrer la nouvelle itération de Red Hat Entreprise Linux pour pouvoir finalement le proposer à nos utilisateurs. Pour cette mission mon interlocuteur principal a été Marc YALAP, mon tuteur qui avait pour rôle ici de valider mon avancement au cours des différentes étapes du projet pour garantir la bonne réalisation de mission.

Voici les objectifs pour cette mission :

- Mise en place d'un miroir RHEL 10 sur l'environnement de LAB

- Création d'une template RHEL 10 sur l'environnement de LAB

- Ajout de l'option RHEL 10 pour le choix de l'OS dans le formulaire de déploiement de VM automatisé

- Ajout de la supervision de RHEL 10 aux différentes automatisations d'audit.

L'échéance de cette mission était le 30 Avril 2026 car à la fin du mois d'Avril est prévue la sortie de la version 26.04 de la distribution Linux Ubuntu, il faut donc finir au plus vite l'intégration de RHEL 10 pour pouvoir préparer celle d'Ubuntu 26.04.

  

Le socle NMS se base sur les solutions de Broad Com VMWare ESXi et VMWare vCenter.

Les machines virtuelles hébergées sur le socle NMS sont coupés d'internet par défaut car la plupart des machines ne doivent pas accéder à internet directement. De ce fait ces dernières, cependant l'infrastructure comprends un proxy fournissant un accès internet. Pour palier ce problème, nous devons dans un premier temps installé un miroir RHEL 10. Un miroir est un serveur stockant et mettant à disposition via les protocoles web (HTTP/HTTPS) des ensembles de paquets de Linux. Pour récupérer ces paquets et les mettre à dispositions au sein de notre environnement local, le miroir devra récupérer les paquets sur internet, pour y accéder, le miroir utilisera le proxy de l'infrastructure qui permet de fournir un accès internet sans exposer directement la machine.

Pour la plupart des distributions Linux, il est possible de récupérer les paquets d'une version supérieur ou même d'une autre distribution, par exemple, il est possible de créer un miroir sur une machine Ubuntu qui distribue les paquets de la distribution Rocky Linux. Hors ceci n'est pas faisable pour les distributions RHEL, en effet, si vous souhaitez créer un miroir RHEL 10 il vous faut une machine RHEL 10, vous ne pouvez pas, sans moyen détourné, récupérer les paquets destiné à RHEL 10 sur un miroir fonctionnant avec RHEL 9.

Pour créer notre miroir RHEL 10, nous allons donc devoir créer dans un premier temps une machine RHEL 10. Dans notre cas, nous avons décidés de partir sur une montée de version de la version 9 vers la version 10. En effet, RHEL 9 étant déjà intégré à notre catalogue d'OS, il me suffit simplement de déployé une machine virtuelle à l'aide de notre automatisation de déploiement pour avoir une machine RHEL 9 fonctionnelle.

Une fois installé, la première étape est d'enregistrer la machine auprès des serveurs de Red Hat, de cette manière notre nouveau système fraîchement déployé pourra accéder aux miroirs officiels de Red Hat et pourra donc y récupérer les paquets nécessaire à la montée de version. Pour cela Red Hat propose un utilitaire fonctionnant en ligne de commande nommé subscription-manager il permet de facilement gérer l'enregistrement des machines et les licences que l'on leur attribue.

Une fois enregistré, il faut donc télécharger les paquets permettant la montée de version, c'est ici que j'ai rencontré le premier point bloquant de la mission. Pour passer de la version 9 à la version 10 de RHEL il existe l'utilitaire Leapp qui s'installe sous la forme d'un paquet disponible sur les miroirs de Red Hat, hors au moment de l'installation de l'utilitaire, celle-ci bloquait et n'installait pas le paquet. Pour résoudre ce problème, je me suis aidé d'une IA interne à SFR ImpAct, celle-ci m'a très rapidement aidé à trouver l'origine du problème ainsi que la solution à ce dernier. L'erreur était causé par l'une des dépendances de Leapp. Les dépendances sont des paquets nécessaires au fonctionnement d'autres paquets. Dans mon cas, la machine possédait une des dépendances de Leapp mais dans une version obsolète, cependant le manager de paquet permet d'installer les paquets ne supprime pas, par défaut, les anciennes versions de dépendances. Une fois cela compris, j'ai pu modifier la commande me permettant d'installer Leapp pour que celle-ci supprime les versions obsolètes de la dépendance problématique.

Après la finalisation de la montée de version, nous supprimons Leapp ainsi que ses dépendances dans le but de garder la machine la plus "propre" possible. Il faut maintenant passer à la mise en place du miroir. Dans un premier temps, il faut créer un nouveau volume logique pour accueillir l'ensemble des paquets qui seront redistribué par le miroir de manière à les isolés du reste du système. Pour faire cela, j'utilise la solution LVM qui permet de gérer des ensembles de disques physiques pour les rediviser dynamiquement en volume logique.

Une fois cela fait, il faut générer un certificat ainsi qu'une clé privé pour permettre la distribution des paquets via le protocole HTTPS. Ce protocole chiffre les échanges en se basant sur un système de clés asymétriques avec une clé privé et une clé publique, la clé public (certificat) va permettre de chiffrer les données et la clé privé va servir à déchiffrer les données une fois reçu ce qui évite que n'importe qui puisse lire les données échangées. Pour ce faire, j'utilise l'utilitaire de ligne de commande OpenSSL qui en une commande me permet de générer les différents éléments nécessaires à la mise en place du HTTPS.

Il faut maintenant installer les différents outils qui nous permettront de mettre en place le miroir en lui-même à savoir :

- httpd : logiciel permettant de créer un serveur web

- mod_ssl : paquet nécessaire à httpd pour l'utilisation du protocole HTTPS

- createrepo : outil permettant de préparer les éléments nécessaires à la création d'un miroir

Une fois les outils installés, il nous faut désormais configurer httpd pour que celui-ci traite les requêtes entrantes et puissent distribuer les différents paquets. Après la configuration, il nous reste à autoriser les requêtes entrantes à passer à travers le pare-feu de la machine pour cela j'utilise l'outil en ligne de commande NF Tables qui s'appuie sur le célèbre IpTables pour gérer les règles de pare-feu. Enfin, il suffit de démarrer httpd pour que le miroir soit opérationnel.

  

Nous venons de réaliser la première grande partie de la mission qui consiste à mettre en place un miroir RHEL 10. La seconde partie consiste à créer une machine RHEL 10 à partir de l'ISO d'installation standard dans le but de créer une template qui servira de base pour déployer les futurs machines virtuelle RHEL 10.

Tout d'abord il faut récupérer l'ISO et la mettre sur l'environnement NMS pour pouvoir créer une machine virtuelle à partir de celle-ci. Une fois fait, nous pouvons commencer l'installation de notre machine, celle-ci requiert une installation particulière, SFR étant un OIV, les systèmes de l'entreprise sont régulièrement audités, il faut donc s'assurer du respect des recommandations ANSSI sur l'ensemble des systèmes. La plupart des éléments recommandés par l'ANSSI sont a réalisé après l'installation initiale de la machine cependant, le découpage du disque système doit absolument être réalisé à l'installation.

Vous pouvez retrouvez les recommandations de l'ANSSI sur le découpage des partitions Linux à l'annexe {{LIEN VERS ANNEXE}}.

Un second point d'attention au cours de l'installation est la création d'un compte utilisateur avec les permissions sudo. Cela évite de définir un mot de passe pour le compte root ce qui est considéré comme une mauvaise pratique de sécurité.

Une fois l'installation terminé, il suffit de tester la connectivité via SSH pour s'assurer que nos automatisations Ansible pourront interagir avec la machine. Après avoir validé la connectivité

  

# Partie 3 : Bilan et recul sur les missions
## Analyse de l'entreprise

L'entreprise SFR est un très grande entreprise

## Apport à l'entreprise
Cette période en entreprise, a été pour moi une occasion de mettre à l'œuvre mes compétences techniques et surtout d'en acquérir de nouvelles. En effet, j'évolue au sein d'une équipe d'ingénierie, ceci implique que la plupart de nos missions sont très grandement orientées vers des aspects techniques. 
  
  

# Conclusion