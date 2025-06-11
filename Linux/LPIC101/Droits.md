#LPIC101
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