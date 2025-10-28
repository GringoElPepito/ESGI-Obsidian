#LPIC101 
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


## Shadow
## Group