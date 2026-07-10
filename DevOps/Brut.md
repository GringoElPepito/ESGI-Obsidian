# Réseau docker
les types de réseaux :
- Bridge - équivalent NAT, réseau par défaut sur docker
	- Le réseau créé par défaut est celui-ci 172.17.0.0/24, puis incrémentation pour les autres réseaux bridge (172.18.0.0/24, 172.19.0.0/24 etc...), il est possible de choisir son propre adressage grâce à l'option `--subnet` , ip par défaut de l'hôte première adresse dispo (172.17.0.1), l'adresse de l'hôte sert de DNS interne à Docker et de gateway.
- host - Récupère l'IP de l'hôte utile pour de la supervision, DHCP, DNS, élément nécessitant beaucoup de configuration réseau ou du broadcast. 
- none (NULL) - pas d'IP
- overlay - utilisé par Docker Swarm
- user-defined bridge
- macvlan (bridge) créer un réseau similaire à un réseau bridge sur VMWare, nécessite que la promiscuité soit activé, n'est pas absolument
- macvlan (802.1q) créer un réseau similaire à un réseau bridge sur VMWare permettant de créer des sous-interfaces par VLAN. Nécessite aussi que le mode promiscuité soit activée. Crééer une adresse MAC virtuelle par conteneur
- IP VLAN (L2/L3) 
	- L2 - comme le macvlan (bridge) mais sans adresse MAC virtuelle
	- L3 - transforme l'hôte en routeur, il change le fonctionnement du réseau docker pour que tous les sous-réseaux existants sur docker soit routé par l'hôte. Ce réseau nécessite la création de règle iptables  pour que l'exposition des services des conteneurs soient fonctionnels

Mappage de port

Lister les types de réseaux utilisés par les conteneurs actifs
```bash
docker network ls
```

Assignation d'un conteneur
```bash
docker run --rm -ti --network
```

```bash
netstat -paunt
```
A noter que les process docker ont un PID à 6 chiffres ou plus

```bash
docker network create -d macvlan \
  --subnet 192.168.1.1
```


# Docker Images & Dockerfile
Chaque LAYER d'une image correspond à une instruction présente dans le docker file

Liste des instructions :
- `FROM` -> image parente
- `LABEL` -> ajout de métadonnées
- `RUN` -> commande(s) utilisée(s) pour construire l'image
- `ENV` -> permet de définir une variable d'environnement par défaut, on appellera les variables de la manière suivante `${nom_de_la_variable}`
- `EXPOSE` -> Port(s) écouté(s) par le conteneur
- `ARG` -> Variables passées comme paramètres à la construction de l'image
- `COPY` -> Permet de copier un fichier présent sur la machine hôte vers le conteneur
- `ADD` -> Fais la même chose que `COPY` mais permet aussi de récupérer des fichiers et archives sur internet et les décompresses si besoin
- `WORKDIR` -> effectue un cd au sein de l'image conteneur
- `VOLUME` -> Créer un point de montage
- `USER` -> utiliser un UID de préférence non-existant sur la machine hôte
- `ENTRYPOINT` -> Commande immuable exécuté au démarrage du conteneur, prend une liste de sting `["ping","--help"]`
- `CMD` -> Commande exécuté au démarrage du conteneur qui peut être remplacé à l'exécution de la commande de démarrage du conteneur
Une bonne pratique est d'utiliser `ENTRYPOINT` pour la commande de base a exécuter et `CMD` pour le ou les arguments de la commande qui pourront donc être surchargé
```Dockerfile

```

Le cache enregistre chacune des couches (Layer) d'une image 
Cache Busting : Mutualiser toutes les commandes dans le moins de `RUN` possible

.dockerignore permet d'ignorer certains fichiers que auraient dû être copié par une instruction `ADD` ou `COPY`. Fonctionne comme un .gitignore

Placer les layers les plus susceptibles d'être modifiés à la fin et les moins susceptibles d'être modifier au début.

Pour une image utilisé en environnement de DEV plutôt favorisé l'utilisation d'un volume pointant vers un dossier de la machine contenant le code de l'application permettant d'éviter un build de l'image à chaque modification du code.

Choisir une image spécifique en X.Y.Z pour s'assurer d'avoir une version stable

## Build Multi-Stage
Le multi-stage consiste à créer une ou plusieurs images intermédiaire dans le but de préparer tous les éléments nécessaires aux fonctionnement de l'application et copier le résultat dans une image finale allégé.

Le multi-staging permet aussi de gérer les différences enter les différents environnement

la target est le nom d'une image intermédiaire on le définit de la manière suivante :
```Dockerfile
FROM apline:latest AS git
```
Le nom suivant l'instruction `AS` est le nom de la target

Il est d'ailleurs possible de réutiliser une des images intermédiaires comme une image de base :
```Dockerfile
FROM git AS builder
```

Il est possible de réaliser le build d'une image spécifique en précisant le target au lancement de la commande de build :
```bash
docker build --target git -t gitimage:1.0 .
```

Pas de build sur un Docker Compose de prod. En production on préférera utiliser un registry privé dans lequel sera assuré le versioning des images

Supprimer les images intermédiaire :
```bash
docker image prune
```

# Docker Compose
Docker compose est un orchestrateur
Un orchestrateur est un gestionnaire de process (Systemd, Kubernetes, Docker-Swarm)
Propriété de base d'un service dans Docker Compose :
- image
- build
- volumes
- restart
- container_name: permet de remplacer le nom par défaut d'un conteneur, par défaut celui-ci prendra le nom suivant "<nom_du_dossier>_<nom_du_service>"
- environment: permet de créer des variables d'environnement sur le format clé valeur au sein du conteneur
```yaml
services:
	user-service:
		build: ./user-service
		container_name: user-service
```

```YAML
services:
	user-service:
		build:
			target:
			context:
			dockerfile:
```

Gestion des variables :
Par défaut, docker-compose se base sur les variables présent dans le fichier `.env`, il est possible de changer cela grâce à la propriété `env_file`
```yaml
services:
	user-service:
		env_file: .env.prod
```

Les variables récupérées de puis le fichier d'environnement sont utilisable de la manière suivante: `${<nom_de_la_variable>`:
```YAML
environment:
	POSTGRES_USER: ${POSTGRES_USER}
```

il est possible de définir une valeur par défaut dans le cas où une variable ne serait pas définit grâce à la syntaxe suivante `:-<valeur_par_defaut>`
```YAML
environment:
	POSTGRES_USER: ${POSTGRES_USER:-user}
```

Gestion des réseaux
Par défaut docker compose créer un réseau en le nommant de la manière suivante `<nom_du_dossier>_<nom_du_reseau>`, ce comportement peut être changé grâce à `external: true` qui permet d'utiliser le réseau déjà existant et d'empêcher l'exécution du docker compose si le réseau n'existe pas.
```YAML
networks:
	todo-network:
		driver: bridge
		external: true
```

Il est possible de fixer des adresses sur les conteneurs :
```
networks:
	to-network:
		ipam:
			
```

Les politiques :

restart -> permet de gérer le redémarrage, il prend 3 valeurs possibles :
- `always` -> redémarre le conteneur peu importe ce qu'il a arrêté
- `on-abort` -> ne redémarre pas le service s'il a été arrêté manuellement
- `on-failure` -> redémarre le conteneur si celui-ci tombe en erreur

depends_on:

```
depends_on:
  - postgres # Vérifie que postgres est running


depends_on:
	service: postgres # Vérifie que postgres est healthy
	service_condition: service_healthy
```

healthcheck -> permet de surcharger le healthcheck présent dans l'image

```
```

Gestion des volumes :
Pour les volumes data, il faut déclarer sa création de la manière suivante
```
volumes:
	postgres-data:
```


Gestion des déploiements
Gestion des ressources:
```YAML
deploy:
	ressources:
		limits:
			cpu: '0.5' # Alloue 50% d'un coeur du processeur
			cpu-shares: ``
			cpuser-cpus: '2' # Permet de spécifier le cpu à utiliser
```


Pour ajouter un registry :
```
```

Pour push une image sur un registry privé:
```
docker tag devimage:1.0 registry.private.com:5000/devimage:1.1
docker push devimage:5000
```

Pipeline commune multi-environnement
Maxer sécu
Doc final + pres

## Pipeline

Coutinuous Delivery -> Le déploiement final se fait via un push button demandant une validation manuelle
Continuous Deployment -> L'entierté du déploiement est automatisé

Le but de la pipeline est de 

Le fichier de base est `.gitlab-ci.yml`  ex :
```YAML
image: alpine:latest

stages:
	- build
	- test
	- deploy

build_job:
	stage: build
	script:
		- echo "Construction de l'application"
		- echo "Remplacer par -> npm run build / mvn package / make..."
		  
test_job:
	stage: test
	script:
		- echo "Exécution des tests..."
		- echo "Remplacer par -> pytest / jest / phpunit.."
```

Les stages sont exécutés de manière séquentiels
Les jobs sont exécutés de manière parallèles

Pour exécuter les différents jobs, Gitlab fait appel à un Runner

Gitflow correspond à la manière dont les branches sont gérés

Règle d'une bonne pipeline :
- Le fichier de pipeline doit être identique pour toutes les branches, il faut donc réussir à mutualiser les pré-requis des différentes branches (environnement) dans un seul fichier. Pour cela, on utilisera des conditions permettant d'exécuter certains jobs dans certains cas précis.

Les types de Runner :
- Runner personnel créer par vous même. Permettra de servir d'intermédiaire entre la pipeline et le serveur de déploiement final (Généralement via SSH).
- Runner Gitlab -> limité à 400 minutes d'exécution.

Instruction de pipeline :
- `before_script` -> Permet d'exécuter des commandes du code avant le script, en cas d'erreur la pipeline ne failera pas. Etape de prépa, gestion des droits récupération de package.
- `after_script` -> Permet d'exécuter du code après la commande script, en cas d'erreur la pipeline ne failera pas. Etape de sauvegarde ou de clean.

On déclenchera la pipeline généralement à la suite d'un commit sur une branche spécifique

Les runners fonctionnent sur une instance virtualisé (VM). Une VM peut contenir plusieurs instance de runner, pour notamment fonctionner par utilisateur.
Les runners ont besoin d'un exécutor les principaux sont :
- Docker -> Démarre le script sous l'image qui lui a été donnée
- Shell -> Démarre le script directement sur la VM 

Pipeline Editor affiche les erreurs de syntaxe du fichier de pipeline.

Il est important de variabiliser, gérer le cache et les artéfacts.

Ils existent 2 types de variables sur GitLab :
- variable
- secret, 


Les variables classiques peuvent être directement déclaré directement dans le fichier `.gitlab-ci.yml`.
Surcharge de variable :
```YAML
image: 

variables:
	APP: nom_de_mon_app
	
job1:
	variables:
		APP: nouveau_nom_de_mon_app
```

Gitlab pré-défini certaines variables :
```
$CI_COMMIT_BRANCH -> nom de la branche courante
$CI_COMMIT_SHA -> hash complet du commit
$CI_COMMIT_SHORT_SHA -> hash raccourci du commit
$CI_COMMIT_REF_SLUG -> indique le nom de la branche
$CI_COMMIT_TAG

$CI_PIPELINE_SOURCE
$CI_PROJECT_NAME
$CI_DEFAULT_BRANCH
```

Il existe plusieurs types de secret :
- variable -> 
- file -> 

Il existe différent mode de visibilité :
- Visible -> Peut être visible dans les jobs
- Masked -> Masqué dans les jobs mais peut être limité (certains caractère spéciaux non-utilisable)
- Masked and Hidden

Il existe plusieurs options :
- Protectet variable -> Permet d'utiliser une variable uniquement dans une
- Expand variable reference -> Permet d'utiliser des variables dans la déclaration d'un secret

Astuce pour passer une clé ssh dans si besoin que tous les secrets soient en masked :
- Suppression des sauts de lignes, encodage en base 64  : `cat id_rsa | tr -d "/n" . | base64`
- On enregistre le résultat dans une variable GitLab
- On décode la variable et on l'enregistre dans un fichier : `echo $VAR | base64 -d > id_rsa`
- Modification des droits sur la clé `chmod 400 id_rsa`

Lorsqu'on met une clé SSH en mode file ou variable, il faut ajouter un saut de ligne à la fin sinon ne fonctionne pas.

OpenBao -> Gestionnaire de Secret


Les variables GitLab sont utilisables dans n'importe quel fichier YAML présent sur le Repo.

Charbonner la partie environnement

Le code doit être à la racine du projet

Docker in docker avec l'image `docker:26.0.0-dind`

```
stages:
build_job:
	image: docker:latest
	tags:
		- deploy
	environment:
		name: prod
		url: http://localhost:8080
	services:
		- name: docker:26.0.0-dind
		  entrypoint: ["dockerd-entrypoint.sh", "--tls=false"]
	stage: build
	variables:
		DOCKER_DRIVER: overlay2
		DOCKER_TLS_CERTDIR: ""
	script:
		- echo "Compilation de l'application..."

test_job:
	image: node:20
	stage: test
	script:
		- echo bonjour
		- echo mlol
		- echo rien
	allow_failure: true
```

Gestion du cache et des artefact :
Permettent de récupérer un élément d'un job pour l'utiliser dans un autre job
le cache est stocké directement sur le runner

Si du cache est généré sur le runner01 et qui doit être utilisé par un job exécuté sur le runner02 alors il y a 2 possibilités :
- Soit le cache est de nouveaux générés pour être utilisé localement
- Soit il y a un stockage partagé entre les runners
Si la pipeline s'exécute sur 2 runners différents il y a 2 possibilité concernant le cache

la configuration du cache se fait au niveau du job

ex :
```YAML
cache:
	paths:
		- node_modules/

cache:
	key: $CI_COMMIT_REF_SLUG # Permet de lié un cache à une branche et de le rendre utilisable pour toutes les prochaines exécutions de la pipeline
	paths:
		- node_modules/
```

Policy de cache :
- Push/Pull, si les packages NPM sont modifiés, régénération du cache et réenregistrement de celui-ci. Valeur par défaut
- Pull, autorise uniquement la lecture
- Push, autorise uniquement la modification du cache existant

Possible de basé la clé de cache sur un fichier :
```YAML
cache:
	policy: pull
	key:
		files: pakacge-lock.json
```

Il est aussi possible d'utiliser des préfixes :
```
cache:
	key:
		files: package-lock.json
	prefix: auth
```