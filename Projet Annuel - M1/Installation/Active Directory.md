# Gestion certificat pour LDAPS

## Demande de certificat
console mmc -> ajouter le module certificat (cible ordinateur local) -> Personnel clique droit -> Toutes les tâches -> Option avancées -> Créer une demande personnalisé -> suivant -> suivant -> Clé CNG & Format PKCS # 10 -> déroulé "demande personnalisé" et cliquer sur propriété :
- Dans général
	- Nom convivial -> FQDN de la machine
- Objet
	- Nom du sujet
		- Pays/région -> FR
		- Etat -> Ile-de-France
		- Ville -> Fontenay-sous-Bois