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

```yaml

```