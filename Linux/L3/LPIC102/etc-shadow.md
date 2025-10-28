les champs sont délimités par des `:`.
Les champs :
- username
- hash du mot de passe en scrypt (salt : `$y$j9T$`).
- Dernier changement de mot de passe, ex = '20109' indique le nombre de jours écoulé au changement du mot de passe  depuis le 01/01/1970.
- Durée minimum du mot de passe, valeur en jour de 00:00 à 00:00, par défaut = 0. Si 0 peut être changer à n'importe qu'elle moment.
- Durée maximum du mot de passe, valeur en jour, par défaut = 99999 soit 273 ans, date prise en compte à partir date de dernière modification du mot de passe.
- Date de warning, valeur en jour, 
- Durée d'inactivité, valeur en jour correspondant au nombre de jours écoulés depuis lequel le compte a été verrouillé (si oublie de changement de mot de passe). par défaut = vide
- Date d'expiration du compte, valeur en jour depuis le 01/01/1970. par défaut = vide
Le salt est une chaîne de caractère généré aléatoirement et qui est ajouté au hashage du mot de passe pour sécurisé
PAM = Pluggable Authentication Module, ce module retire le salt 
Légalement obligé de hasher les mot de passes en BDD.
epoch ou temps UNIX, temps de référence = 1er Janvier 1970

```bash
date -d "1970/01/01 +20109 days"
```

[^1]: 
