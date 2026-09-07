# Cloud Computing

Serveur à l'échelle de la planète accessible à travers le monde

CSP Cloud Service Provider

Exposé sous forme d'API


# Sécurité informatique

La valeur d'une entreprise se base sur les données qui lui appartiennent.
Il faut donc sécuriser l'information.
Sécurité de l'information :
- SSI -> Sécurité du système d'information, aborde une vision stratégique que l'entreprise doit suivre pour préserver les données
	- Gouvernance
	- Gestion
	- RGPD
	- Sécurité des Bureaux
	- gestion du personnel
	- sensibilisation des employés
	- chartes informatiques
	- politiques de sécurité (PSSI)
- Cybersécurité, vision externe et opérationnelle, contré les menaces

SSI et Cyber travaillent conjointement pour sécuriser l'infrastructure de l'entreprise. Le SSI et la Cyber ont pour but d'aboutir à la sécurité informatique.

La sécurité informatique correspond à la partie technique.

DICT
- Disponibilité, service ou donnée doit être disponible
- Intégrité, la donnée ne doit pas être modifié de manière involontaire ou dérobé
- Confidentialité, seule personnes désignés doivent pouvoir accéder à la donnée
- Traçabilité, pouvoir suivre les actions réalisés et identifié la personne qui en est à l'origine


Protocole AAA
- Authorization
- Authentification
- Accounting

CIA 
- Confidentiality
- Intégrité
- Availability


CDN -> Serveurs de proximité positionner à travers le monde (aux plus proches des utilisateurs) servant à réduire le temps de réponse

Disponibilité :

Intégrité :
Mettre en place de l'alerting pour les fichiers ne devant pas être modifié régulièrement
Sauvegardes récurrentes sur des supports différents

Imputabilité -> Qui a fait l'action ?
Non répudiation -> Peut-elle nier l'avoir fait ?

Traçabilité :

Responsabilité est partagé entre le Cloud Provider et vous-mêmes
- Vous 
	- Data client
	- Platform, app, identité, gestion des accès
	- OS, réseau, FW
- AWS
	- Serveur physique


Les défis de la sécurité de l'information, cyber-défis de sécurité dans le cloud :
- Gouvernance
- Résilience
- Ingénierie social
- Automatisation (DevSecOps)
- Environnement en constante évolution
- Mauvaise configuration
- Gestion des privilèges
- Sécurisation des données
- Gestion de la surface d'attaque/Rayon d'explosion
- Manque de visibilité


Les 3 leviers de sécurisation
- Outils
	- Interne -> Mis à disposition par les clouds providers nombre outils libre à l'exploitation
	- 
- Compétences
	- Formation du personnel
- Processus


Zero Trust :
- Segmentation 
	- Isolation logique des ressources entre elle afin d'éviter le pivoting (capacité pour un attaquant de bondir de ressource en ressource).
	- Au travers de politique de sécurité et de segmentation réseau.
- Controle d'accèes
	- Moindre privilège
	- Accès temporaire
	- Authentification (+MFA/2FA)
	- Minimum de droit et le bon nombre droit
	- Politique d'authentification et d'autorisation granulaire
- Détection des menaces
	- Détection des intrusions
	- Solution expiration des logs inaltérables
	- Imputabilité

Gestion image pour VM :
Image Originale -> Golden/Base Image -> Image Applicative

Packer -> pour hardener une image

Image applicative 


SOAR

SOC (Security Operation Center)

CSIRT (Computer Security Incident Response Team)