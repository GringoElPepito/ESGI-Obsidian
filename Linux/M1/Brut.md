# Mesure de l'utilisation des ressources
Il existe 2 commandes pour surveiller la consommation de ressources de notre machine :
- `top` -> 
- `htop`  (pour human `top`) -> 

`htop` permet de tuer un service en cours de fonctionnement.

# Analyse des ports 
Il existe 2 commandes pour analyser les ports connecté ou en écoute :
- `netstat` 
- `ss` -> nouvelle version de `netstat`

elles affichent plusieurs informations comme
- Le protocole utilisé `tcp` ou `udp` généralement
- Le nombre de paquets reçus
- Le nombre de paquets envoyés
- L'adresse IP et le port local
- L'adresse IP et le port distant si en communication, sinon les adressages et ports acceptés
- L'état du port
	- `LISTEN` : Le port est en écoute
	- `TIME_WAIT` : Après la fermeture d'une session, le port se met dans cette état pour reprendre la connexion rapidement en cas de coupure involontaire
	- `ETABLISHED` : connexion établie
- Le processus
- 