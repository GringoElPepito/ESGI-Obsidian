# Histoire de Linux
Linux n'est pas un OS (operating system), Linux est une partie d'OS, c'est le noyau d'un système d'exploitation.
Debian, Ubuntu, Fedora… = GNU/Linux
GNU combinés à Linux

Multics.

Fin des années 60 (1969), UNIX est né au laboratoires Bell, créé par Dennis RITCHIE et Kenneth THOMSON, ils sont aussi les créateurs du langage C.
Unix vise à combler les défaut du Multics.

Aujourd'hui, Processeurs Intel et AMD. Architecture 64bits (amd64). amd64 correspond à la taille des jeux d'instructions et des registres CPU.

Un programme fonctionne par architecture processeur.

Processeurs M1, M2 et M3 : Architecture ARM64.

de ce fait un programme fait pour l'architecture amd64 ne pourra pas être exécuter sur un processeur arm64

chaque éditeur de UNIX avait sa propre version de UNIX, cela était dû au fait que chaque éditeur avait son propre matérielle. 

Par la suite, Intel s'est imposé ce qui a permit par la suite d'unifier les système.

RMS => Richard Mattew Stallman, il souhaitait modifier le driver de son imprimante Xerox, mais son système ne le lui permettait pas. Il a donc décidé de créer un équivalent UNIX open-source et gratuit qui pourra être modifier par n'importe qui.

GNU (GNU is not UNIX). La mascotte est un GNOU.

UNIX libre et open-source. FOSS (Free and Open-Source Software).
=> ls, cd, mkdir, cp, mv...

Le noyait manquait à GNU. Fonctionnalités principales sont les suivantes :
- Interface entre le matériel et le logiciel.
- Gestion et ordonnancement de processus.

La partie noyau et la partie espace utilisateur.

Linux => Noyau 

Linux Torvald a créé Linux qui devait s'appeler Freax.

Linus Torvald => Linux
RMS => GNU

Le noyau Linux et le système GNU a donné le système GNU/Linux.

Distributions GNU/Linux => Debian, Ubuntu, Fedora, Redhat, Arch Linux...

Paquets => un logiciel distribué via un serveur Web.

Interface graphique (GNOME, KDE...)
Navigateurs Web (Firefox)
Lecteurs multimédia (VLC...)

Une distribution est une extension du système GNU/Linux distribué une version de GNU/Linux avec des des logiciels supplémentaires pour étendre les fonctionnalités du système.

 **Types d'utilisateurs**
Utilisateurs standards pouvant se connecter
Utilisateurs root (super admin sur Windows)

**Alias**
alias c=clear permet de taper `c` pour exécuter la commande `clear`

FHS = File Hierarchy Standard
tmpfs=Temporary Filesystem est système de fichier temporaire 

Interdiction de mettre 777 en droit sur un répertoire ou un fichier.

Commande pour récup le nombre de cœur d'un processeur.
![[Pasted image 20241021151041.png]]
# Répertoire Linux
/ est la racine et contient nativement les dossiers suivants :
- /bin : est un lien symbolique pointant vers /usr/bin qui stocke les binaires utilisateurs
- /sbin : est un lien symbolique pointant vers /usr/sbin qui stocke les binaires systèmes (System binaries). Pour exécuter les commandes dans ce dossier il faut être en root ou en sudo
- /boot : stocke les fichiers servant au démarrage du système, contient le chargeur d'amorçage.
- /dev : Stocke tous les périphériques matériel connecté physique ou logique à la machine. Pseudo devices sont les fichiers apparaissant dans ce répertoire qui ne correspondent pas à des périphériques.
- /etc : Editable Text Configuration, stocke l'intégralité des fichiers de configuration du système. L'intérêt de ce répertoire est que les logiciels regardent directement dans ce dossier pour vérifier s'il n'y a pas déjà une configuration existante.
- /home : Contient tous les répertoires personnels de chaque utilisateurs
- /lib et autres : Contient des fichier `.so` (Share Object) qui sont des librairies permettant aux autres applications/logiciels de fonctionner
- /media : Utilisé par les interfaces graphiques pour monter les périphériques externes type CD-ROM, Clé USB, Disque Dur etc...
- /mnt : Mount, répertoire dédié au montage des périphériques via CMD
- /opt : Optional, répertoire servant soit à stocker des scripts soit des programmes non-standard (généralement ceux ayant besoin de tous les fichiers au même endroit que l'exécutable).
- /proc : est un *tmpfs*, ce répertoire est directement géré par le système, il centralise les informations de fonctionnement processeur et des différents programmes tournant sur la machine.
- /root : dossier personnel du compte root
- /run : est un *tmpfs*, va stocker les fichier PID (Process Identifiant)
- /srv : Service, sert à stocker les données utilisées par les services
- /sys : System, fait le lien entre le matériel et le noyau, contient les infos liés au matériel de la machine.
- /tmp : répertoire vide servant à stocker des fichiers temporaires et étant vidés à chaque redémarrage. Ce répertoire est dit World Writable qui signifie que tout le monde peut tout faire dans ce répertoire. De plus le dernier caractère de droit qui devrait être un `x` ou un `-` est ici un `t`, on appelle cela un Sticky bit est permet d'ajouter une sécurité supplémentaire en autorisant pas la suppression ou modification d'un fichier créer par un autre utilisateur (sauf si root).
- /usr : (Unix System Ressources), initialement le répertoire des utilisateurs. Désormais est un répertoire un peu fourre-tout qui sert plus ou moins de coeur au système
	- /usr/share : contient la quasi-totalité des fichiers spécifiques aux distributions
- /var : on y retrouvera différents répertoire comme le cache, local, log, backups, www etc...

Sticky bit = Protéger les fichiers des utilisateurs. Par exemple, un fichier sur lequel l'utilisateur toto ne sera pas propriétaire ne pourra supprimer un élément dans le dossier ayant le sticky bit

Les DAC = Discretionary Access Control,
Les droits sont à la charge du propriétaire du fichier||dossier
Les types d'utilisateur dans les droits d'un fichier
- Utilisateur propriétaire
- Groupe propriétaire
- Les autres

Les droits DAC dans un fichier
- Lecture = 4 = r
- Ecriture = 2 = w
- Exécution = 1 = x

Sur un dossier le droit de lecture permet d'effectuer un ``ls`` dessus et le droit d'exécution permet de ce déplacer dedans ``cd``. Il faut donc pour un dossier qui doit être en lecture il faut mettre les droits en lecture et en exécution soit 5

raccourci d'affectation chmod
- u = user
- g = group
- o = other
- a = all

```bash
sudo chmod ug-r fichier #Retire les droits de lecture à l'utilisateur et groupe propriétaire
sudo chmod u=r fichier #Change tous les droits du propriétaire pour ne lui laisserque le droit de lecture
sudo chmod a+rx fichier #Ajoute les droits de lecture d'excution à tous le monde
sudo chmod -x fichier #Retire le droit d'execution a tout le monde
```

#setuid #suid #capabilities
Le x peut être remplacé par s dans l'affichage des droits, cela correspond au setuid qui permet aux utilisateurs de modifier un fichier comme s'il avait les mêmes droit que le propriétaire.
il peut y avoir un second s, qui correpond au setgid
Le sticky bit noté t permet de restreindre les actions d'un utilisateur a sur un fichier créer par l'utilisateur b.

Pas mettre de mot de passe a root pour créer un utilisateur avec sudo
# Les métacaractères
`*` est un joker permettant de remplacer toutes les occurrences possible
`?` est un joker remplaçant un seul caractère
`[0-9]` permet de prendre en compte n'importe quel chiffre compris entre 0 et 9 inclus

# Fichier utilisateur/groupe
## Passwd
kaisen:x:1000:1000:Kaisen,,,:/home/kaisen:/bin/bash
- kaisen = username
- x = placeholder pour remplacer le mot de passe qui était anciennement stocké dans ce fichier
- 1000 = UID User ID
- 1000 = GID Group ID
- Kaisen,,, = Champ commentaire
- /home/kaisen = path to home directory
- /bin/bash = path to the shell, le shell doit être chemin absolu sinon il n'est pas reconnu
Pour modifier les propriété d'un utilisateur on utilisera `usermod`
root ayant l'UID 0, si on créer un utilisateur avec l'UID 0 à sa connexion celui-ci se connectera en tant que root.
Possibilité de passé root via vim
```bash
sudo vim fichier
:shell
```


## Shadow
## Group

## LS -lhi
```bash
784925 -rw-r--r-- 1 kaisen:kaisen 4.0K Nov 19 10:18 fichier
```
- 784925 : inode
- -rw-r--r-- : droit du fichier
- 1 : nombre d'inode
- kaisen:kaisen : propriétaire:groupe
- 4.0K : nombre de Byte
- Nov 19 10:18 : Date de dernière modification
- fichier : nom du fichier
`ls -A` : affiche tous les fichiers mais n'affiche pas `.`  & `..`
`ls -a` : affiche tous les fichiers

# Device
sda = SCSI Device a


# Gestionnaire de paquet APT
APT est une surcouche d'abstraction utilisant DPKG en arrière-plan. La différence est que DPKG ne fait pas de résolution de dépendances. C'est APT qui s'en occupe 
Met à jour la liste des paquets téléchargeables mais ne mets pas à jour les paquets
```bash
sudo apt update
```
Met à jour les paquets sans régler les soucis de dépendances
```bash
sudo apt upgrade
```
Met à jour les paquets en réglant les soucis de dépendances
```bash
sudo apt full-upgrade
```
Réparer les problèmes de dépendances
```bash
sudo apt install -f
```
Lister les paquets disponible sur les repos
```bash
sudo apt list
```
Lister les paquets installés avec DPKG
```bash
dpkg -l
```
Supprimer un paquet
```bash
sudo apt remove
```
Supprimer un paquet et ses dépendances si elles ne sont pas utilisés
```bash
sudo apt autoremove
```
Supprimer un paquet, ses dépendances non utilisés et les fichiers de configuration
```bash
sudo apt autoremove --purge
```
Supprimer les configurations d'un paquet désinstallé
```bash
sudo dpkg --purge nom_du_paquet
```