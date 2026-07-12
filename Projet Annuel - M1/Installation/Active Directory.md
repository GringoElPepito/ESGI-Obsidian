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
		- Organisation -> CENEXIS
		- Unité d'organisation -> Community
		- Adresse de messagerie -> admin@cenexis.lan
		- Nom commun -> FQDN de la machine
	- Autre nom
		- DNS -> nom de la machine (sans le domaine)
		- DNS -> FQDN de la machine
		- Adresse IP (v4) -> IPv4 de la machine
- Extensions
	- Utilisation de la clé
		- Signature Numérique
		- Chiffrement de la clé
	- Utilisation de la clé étendue
		- Authentification du serveur
Suivant -> Choisir emplacement fichier de requête -> terminer

Transférer le fichier sur sub-ca.cenexis.lan via SSH dans le dossier `/home/desigual/sub-ca/req`
lancer la commande suivante :
```bash
openssl ca \
-batch \
-config sub-ca.conf \
-subj "/CN={FQDN}/OU=Community/O=CENEXIS/L=Fontenay-sous-Bois/S=Ile-de-France/C=FR" \
-create_serial \
-in req/hostname.req \
-out certs/hostname.crt \
-extensions server_ext \
-days 365 \
-passin pass:{MDP_SUB_CA_PRIVATE_KEY}
```

Enfin transfert et ajout du certificat sur le serveur AD