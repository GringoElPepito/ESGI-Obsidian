#LPIC101
`/` est la racine et contient nativement les dossiers suivants :
- `/bin` : est un lien symbolique pointant vers /usr/bin qui stocke les binaires utilisateurs
- `/sbin` : est un lien symbolique pointant vers /usr/sbin qui stocke les binaires systèmes (System binaries). Pour exécuter les commandes dans ce dossier il faut être en root ou en sudo
- `/boot` : stocke les fichiers servant au démarrage du système, contient le chargeur d'amorçage.
- `/dev` : Stocke tous les périphériques matériel connecté physique ou logique à la machine. Pseudo devices sont les fichiers apparaissant dans ce répertoire qui ne correspondent pas à des périphériques.
- `/etc` : Editable Text Configuration, stocke l'intégralité des fichiers de configuration du système. L'intérêt de ce répertoire est que les logiciels regardent directement dans ce dossier pour vérifier s'il n'y a pas déjà une configuration existante.
- `/home` : Contient tous les répertoires personnels de chaque utilisateurs
- `/lib` et autres : Contient des fichier `.so` (Share Object) qui sont des librairies permettant aux autres applications/logiciels de fonctionner
- `/media` : Utilisé par les interfaces graphiques pour monter les périphériques externes type CD-ROM, Clé USB, Disque Dur etc...
- `/mnt` : Mount, répertoire dédié au montage des périphériques via CMD
- `/opt` : Optional, répertoire servant soit à stocker des scripts soit des programmes non-standard (généralement ceux ayant besoin de tous les fichiers au même endroit que l'exécutable).
- `/proc` : est un *tmpfs*, ce répertoire est directement géré par le système, il centralise les informations de fonctionnement processeur et des différents programmes tournant sur la machine.
- `/root` : dossier personnel du compte root
- `/run` : est un *tmpfs*  va stocker les fichier PID (Process Identifiant)
- `/srv` : Service, sert à stocker les données utilisées par les services
- `/sys` : System, fait le lien entre le matériel et le noyau, contient les infos liés au matériel de la machine.
- `/tmp` : répertoire vide servant à stocker des fichiers temporaires et étant vidés à chaque redémarrage. Ce répertoire est dit World Writable qui signifie que tout le monde peut tout faire dans ce répertoire. De plus le dernier caractère de droit qui devrait être un `x` ou un `-` est ici un `t`, on appelle cela un Sticky bit est permet d'ajouter une sécurité supplémentaire en autorisant pas la suppression ou modification d'un fichier créer par un autre utilisateur (sauf si root).
- `/usr` : (Unix System Ressources), initialement le répertoire des utilisateurs. Désormais est un répertoire un peu fourre-tout qui sert plus ou moins de coeur au système
	- `/usr/share` : contient la quasi-totalité des fichiers spécifiques aux distributions
- `/var` : on y retrouvera différents répertoire comme le cache, local, log, backups, www etc...

FHS = File Hierarchy Standard


```bash
784925 -rw-r--r-- 1 kaisen:kaisen 4.0K Nov 19 10:18 fichier
```
- 784925 : numéro d'inode*
- `-` : type de fichier
	- `-` : fichier classique
	- `l` : lien symbolique
	- `d` : dossier
	- `c` : caractère
	- `b` : bloc
- `rw-` : droit du propriétaire du fichier
- `r--` : droit du groupe propriétaire du fichier
- `r--` : droit des autres (tout utilisateur n'étant pas propriétaire et ne faisant pas partie du groupe propriétaire)
- 1 : nombre d'inode ou lien physique
- kaisen:kaisen : propriétaire:groupe
- 4.0K : nombre de Byte
- Nov 19 10:18 : Date de dernière modification
- fichier : nom du fichier

[^1]: 

[^2]: 
