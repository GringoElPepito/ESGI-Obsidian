les champs sont délimités par des `:`.
Les champs :
- username
- hash du mot de passe en scrypt (salt : `$y$j9T$`).
- Dernier changement de mot de passe
Le salt est une chaîne de caractère généré aléatoirement et qui est ajouté au hashage du mot de passe pour sécurisé
PAM = Pluggable Authentication Module, ce module retire le salt 
Légalement obligé de hasher les mot de passes en BDD.