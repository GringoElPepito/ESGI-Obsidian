`cd` : permet de changer de répertoire courant
```bash
cd /home
```

`ls` : permet de lister les fichiers contenu dans un dossier, liste des arguments
- `-A` : affiche tous les fichiers mais n'affiche pas `.`  & `..`
- `-a` : affiche tous les fichiers aussi ceux cachés (fichier dont le nom commence par `.`)
- `-l` : affiche les informations liés au fichier (type de fichier, droits, )
- `-h` : affiche les tailles des fichiers dans une unité lisible
- `-i` : affiche le numéro d'inode du fichier
```bash
ls -lhi
784925 -rw-r--r-- 1 kaisen:kaisen 4.0K Nov 19 10:18 fichier
```
`alias` : permet de créer un alias pour une commande 
```bash
alias c=clear
c
```
`cat /proc/cupinfo | grep -i ^processor | wc -l` : permet de connaître le nombre de cœur du processeur d'une machine 

