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
