Fiche récap :
![[16964126324398_Downloadable Summary [Git et Github] .png]]
# Présentation de Git & Github
Git & Github sont des gestionnaires de versions, qui est un programme permettant de conserver un historique des modifications et des versions de tous les fichiers. Il va enregistrer :
- Chaque modification de chaque fichier contenu de le dossier
- La raison de cette modification
- L'auteur de cette modification
La différence entre les 2 est se fait sur leur fonctionnement, Git est décentralisé alors que Github est centralisé. Avec Git le gestionnaire est gérer directement en local sur l'ordinateur ce qui fait que chaque copie du dépôt est indépendante et possède son propre historique. A l'inverse, Github va lui centralisé l'historique en ligne permettant donc de synchroniser toutes les instances du dépôt ce qu'on appelle un dépôt distant.
Cela a plusieurs avantage comme pouvoir récupérer son code depuis n'importe quelle machine, facilité le travail d'équipe et permettant à tous d'y accéder en permanence etc...
On combine généralement l'utilisation de Git et de Github pour facilité la gestion du dépôts au niveau local et distant.
![[16334576106761_image27.png]]
On différencie le dépôt local Git et le dépôt distant Github.
Le dépôt local se divise en 3 parties :
- Le working directory, c'est le dossier du projet sur l'ordinateur.
- Le Stage ou index, c'est une zone intermédiaire entre le working directory et le repository, contenant les fichiers modifiés que l'on souhaite voir apparaître dans la prochaine version du code
- Le Repository ou Dépôt qui est la zone qui va stocker la dernière version active du projet
# Démarrez un projet Git/Github
## Configuration Git
permet de configurer les informations pour les dépôts
```cmd
git config
``` 
En ajoutant le paramètre ``--global`` à la commande, la configuration fourni deviendra celle par défaut et sera donc appliqué sur tous les dépôts si aucune autre configuration est appliqué à la place. 
```cmd
git config --global
```
Voici quelques configurations de base
```cmd
git config --global user.nam "John Doe" #nom d'utilisateur
git config --global user.email johndoe@example.com #email
git config --global color.diff auto #active couleur des différences
git config --global color.status auto #active couleur du statut
git config --global color.branch auto #active couleur des branches
git config --global core.editor vim #ide par défaut
git config --global merge.tool vimdiff #outil de fusion
```
``git config --list`` permet de lister la configuration active sur un dépôt
``git init`` dans le dossier du projet, permet d'initialiser un dépôt git
``git add index.html styles.css`` permet d'ajouter les fichiers ***index.html*** et ***styles.css***
``git add .`` ajoute directement tous les fichiers contenu dans le dossier
Avec ``git add`` les fichiers sont dits indexés. Une fois qu'ils le sont vous pouvez les créer une version, ce qui revient à archiver le projet avec ces fichiers nouvellement ajoutés grâce à :
```cmd
git commit -m "Ajout des fichiers html et css de base"
```
le paramètre ``-m`` permet d'ajouter un message spécifiant des détails sur le commit (création de version) venant d'être effectué.
Pour actualiser le dépôt distant, on va push notre dépôt local pour cela on va d'abord définir le dépôt distant :
```cmd
#configuration en SSH
git remote add origin git@github.com:{Username}/{Repository_name}.git
-----------------------------------OU-----------------------------------
#configuration en HTTPS
git remote add origin https://github.com/{Username}/{Repository_name}.git
```
Après ça on peut finalement push le dépôt local vers le dépôt distant :
```cmd
git branch -M main #permet de sélectionner la branch que l'on veut push
git push -u origin main #initie le push vers le dépôt distant
```
l'argument ``-u`` n'est nécessaire que lors du tout premier push et peut être omis par la suite
# Système de branches
La branche principale est nommé main ou master, pour tous les dépôts Github créé après octobre 2020 est forcément main.
Chaque branche fonctionne comme un dossier différent, une version distincte de votre projet. Ainsi vous pouvez mener des modifications sans directement agir sur la branche principal au risque de cassé une version fonctionnel de votre projet. Il vous suffira d'appliquer les modifications de vos branches sur la branche principal une fois que celles-ci aient été testés et validés.
```cmd
git branch #permet d'afficher la liste des branch existante sur un dépôt
```
S'il n'y a aucune branch ayant été créer alors le résultat sera :
```cmd
* main
```
L'étoile indique la branche où nous sommes actuellement situé.
Vous pouvez créer un branch puis basculer sur celle-ci avec :
```cmd
git branch test #créer la branche
git checkout test #sélectionne la branche
```
Vous pouvez bien évidemment commit et push sur cette nouvelle branche :
```cmd
git commit -m "Réalisation de la partie de test sur le projet"
git push -u origin test
```
Enfin pour appliquer les modifications de la branche test sur la branche main, il faut fusionner (merge en anglais) les deux branches :
```cmd
git checkout main
git merge test
```

# Travaillez avec un dépôt distant
Il est possible de cloner un dépôt distant en local grâce à :
```cmd
#en SSH
git clone git@github.com:{Username}\{Repository_name}.git
#en HTTPS
git clone https://github.com/{Username}\{Repository_name}.git
```
Pour mettre à jour le dépôt local :
```cmd
git pull origin main
```
S'il existe différente branch sur le dépôt distant alors elles seront elles aussi copiés en local. Dans le cadre d'un travail d'équipe sur un dépôt dont on est pas propriétaire, on ne va pas directement fusionner les différentes branches avec la branche main. On va devoir demander à ce que notre code soit ajouter à la branche principal avec une pull request (demande de tirage en français).
Il faut créer une nouvelle branche et y commit les modifications. Après cela il faut se rendre sur Github et se laisser guider par l'interface pour y formuler une pull request.
# Corrigez les erreurs sur un dépôt local
Pour supprimer une branche :
```cmd
git branch -d test
```
Si la branche contient des modifications que vous souhaitez aussi supprimer alors :
```cmd
git branch -D test
```
Pour annulé des modifications apportées sur la branche principal mais pas encore commit et les transmettre sur une autre branche on utilise ``stash`` (remise en français) :
```cmd
git stash #retire les modifications de la branche actuelle
git branch brancheCommit
git checkout brancheCommit
git stash apply # applique le dernier stash réalisé sur la branche sélectionné
```
Si vous avez réalisé plusieurs ``stash``, on peut les lister avec :
```cmd
git stash list
```
Pour appliquer un ``stash`` spécifique :
```cmd
git stash apply stash@{0}
```
Pour lister les commits du plus au moins récents :
```cmd
git log commit
```
Pour supprimer complètement le dernier commit appliqué :
```cmd
git reset --hard HEAD^
```
Pour supprimer un commit spécifique, il faut récupérer les 8 premiers caractères de l'identifiant du commit visible avec ``git log`` puis utiliser cette commande :
```cmd
git reset --hard ca83a6df #ca83a6df est le début de l'identifiant
```
Pour modifier le message d'un commit déjà fait :
```cmd
git commit --amend -m "Nouveau message du commit"
```
S'il manque un fichier dans un commit :
```cmd
git add leFameuxFichierOublie.txt
git commit --amend --no-edit
```

# Corrigez les erreurs sur un dépôt distant
Pour supprimer un commit sur un dépôt distant  ( équivalent à un ``git reset`` ):
```cmd
git revert HEAD^
```
Cette commande va créer un nouveau commit supprimant les changement précédemment réalisé. N'ayant ainsi aucun impact sur l'historique.
``git reset`` existe sous 3 formes ``--soft``, ``--mixed``, ``--hard``
![[16334774544128_image2 1.png]]
La version ``--hard`` va revenir au commit ciblé et supprimer tout ce qu'il s'est passé avant
```cmd
git reset commitCible --hard
```
La version ``--midex`` revient au dernier commit ou dernier commit spécifié sans enlever les modifications en cours. Il permet aussi de désindexer les fichiers qu'ils le sont et qui n'ont pas encore été commités. C'est aussi la valeur par défaut si aucun mode n'est spécifié.
Enfin la version ``--soft`` permet de revenir à un commit spécifique afin de voir le code à un instant précis ou de créer une branche en partant d'un ancien commit. Elle ne supprime aucun fichier, aucun commit et ne créer pas de ``HEAD`` détaché.
``HEAD`` est un pointeur, référençant la position actuelle dans le répertoire de travail Git. Par défaut elle est sur la branche courant main/master. Mais elle se déplace lorsque que vous vous déplacez.
S'il un merge donne lieu à un conflit, Git va se charger de stopper la fusion des fichiers et de mettre en évidence l'origine du conflit. Il faudra ensuite ouvrir le ou les fichiers en conflits arbitrer sur le contenu à garder et celui qui doit être retiré puis les enregistrer et les indexés avec ``git add``.
La différence entre ``git revert`` et ``git reset`` est que le premier va créer un nouveau commit en comparant les différences entre le dernier et l'avant dernier commit et appliquant les modifications nécessaires pour revenir à l'état voulu. Préservant ainsi l'historique de modification. A l'inverse, le second revient au commit précédent sans créer de nouveau commit.
![[16334778030431_image19.jpg]]

# Corrigez un commit raté
Git enregistre des logs de toutes les modifications apportés, elles sont consultables via la commande ``git log`` qui énumère dans un ordre chronologique inversé les commits réalisés. Les commits les plus récents apparaissent en premier.
``git reflog`` va loguer les commits et toutes les autres actions (modifications de messages, merges, resets). Pour revenir à un état en antérieur on prend les 8 premiers caractères de l'identifiant de l'action puis on réalise un ``git checkout``.
Pour vérifier en détail toutes les actions mener sur un fichier il existe la commande :
```cmd
git blame index.html
```
cette commande affiche pour chaque ligne modifié :
- ID,
- Auteur,
- Horodatage,
- Numéro de ligne,
- Contenu de la ligne
Pour appliquer un commit d'une branche à une autre on peut utiliser ``git cherry-pick`` qui permet de migrer des commit d'une branche à une autre. exemple :
```cmd
git cherry-pick d356940 de966d4
```
cette commande va ajouter les commits avec l'identifiant SHA d356940 et de966d4 à la branche principal sans les enlever de leur branche d'origine.