cgroups fonctionnalité du kernel linux permettant d'isoler les processus avec un espace mémoire dédié
namespace fonctionnalité du kernel permettant de donner un nom au processus et de les identifier grâce à celui-ci.

Image Docker : Equivalent d'un OVA pour les conteneurs
Conteneur Docker : Equivalent de la VM, résultat du lancement d'une image.

Docker Hub est un registre de conteneur qui stocke les images docker.

Docker Commande de base
```bash
docker 
```

Récupérer l'image docker NGINX :
```bash
docker pull nginx
```

Lister les images docker :
```bash
docker images
# ou 
docker image ls
```

Créer un conteneur à partir d'une image :
```bash
docker run nginx
```

Créer un conteneur à partir d'une image sans bloquer le terminal :
```bash
docker run -d nginx
```

Créer un conteneur à partir d'une image en nommant le conteneur, il n'est pas possible d'avoir deux conteneurs avec le même nom :
```bash
docker run --name nginx nginx
```

Créer un conteneur à partir d'une image en mappant les ports :
```bash
docker run -p 80:80 nginx
```
`-p` permet de définir la correspondance des ports entre la machine hôte et le conteneur, `80:80` le `80` situé à gauche du `:` correspond au port de la machine hôte et le `80` est le port du conteneur.

Lister les conteneurs docker en cours d'exécution :
```bash
docker ps
```

Lister tous les conteneurs docker aussi ceux arrêté :
```bash
docker ps -a
```

Stopper un conteneur
```bash
# Avec le conteneur ID
docker stop 909392e75bf3
# Avec les 2 premiers caractère du conteneur ID
docker stop 90
# Avec le nom du conteneur
docker stop nginx
```

Pour relancer un conteneur arrêté : 
```bash
# Avec le conteneur ID
docker start 909392e75bf3
# Avec les 2 premiers caractère du conteneur ID
docker start 90
# Avec le nom du conteneur
docker start nginx
```

Supprimer un conteneur lorsqu'il s'arrête :
```bash
docker run -d --name nginx-kevin --rm nginx
```
