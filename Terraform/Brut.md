vérifier qu'un compte est bien connecté à AWS-CLI
```bash
aws sts get-caller-identity
```

Lorsqu'un fichier est en cours de modification sur un stockage S3, celui-ci est verrouillé.

AWS
Compte -> Un conteneur étanche de ressources et de factures identifié par douze chiffres.

Dans le lab : un compte par étudiant


La région
us-east-1, Virginie du Nord partout dans le module.
Six zone de disponibilité, de us-east-1a à us-east-1f

L'identité 


Une instance, quatre décision
AMI -> Image de départ son id change par région et par version.

Type et clé
t3.micro : 2 vCPU, 1Gio
AWS ne garde que la clé publique ; la clé privée reste chez vous.

user_data
script lancé par cloud-init, en root, pour configuré l'instance EC2 à son déploiement


Security-Group ensemble de règle réseau permettant de gérer les flux entrant et sortant d'une instance EC2. Typiquement autorisation du SSH. Par défaut tout est refusé


# Terraform

Inputs -> variables.tf, ce que l'appelant fournit. Typé, validé, documenté

Corps -> main.tf, les resources créées. Invisibles depuis l'extérieur.

Outputs -> outputs.tf, les


Appeler un module
Le bloc module porte un nom local (web) et une source.
Les arguments sont les variables du module.
On lut ses outputs avec module.nom.output
terraform

```tf
module "web" {
}
```

Récupération du module :
- Local
- Git + Tag
- Registre public

Deux règles qui ne se discutent pas
1. Un module enfant ne déclare jamais de bloc provider : il l'hérite du module racine. Seulement required_providers avec des contraintes de version.
2. Un module d'une seule ressource sans variable est un signal d'alerte : de l'indirection sans bénéfice.

Gestion des environnements DEV et PROD
Option 1 -> Workspace
```bash
terraform workspace new dev
terraform workspace new prod
terraform select prod
```

Option 2 -> Un dossier par environnement :
- Infra
	- modules
		- aws
	- envs
		- dev
			- backend.tf
			- main.tf # ref=v1.1.0
			- terraform.tfvars
		- prod
			- backend.tf
			- main.tf
			- terraform.tfvars


Deux environnements dans un seul compte
Les tags, default_tags :
- Project
- ManagedBy

Gérer les states séparément

## Secrets & IAM
les point de fuite d'un secret :
- Le code
- Le state
- user_data
- Les logs

Où stocker
- SSM Parameter Store
- Secrets Manager -> Rotation, RDS (réplication)

Ecrire un secret sans qu'il soit enregistrer dans le state
```tf
variable "db_password" {
	type = string
	ephemeral = true
}

resource "aws_ssm_parameter" "db_password" {
	name = "/novasphere/$(var.environment)/db_password"
	type = secure_string
	value_wo = 
	value_wo_version = 1
}
```


checkov -> analyse du code HCL
trivy -> scanne les images et les dépendances


IAM -> en quatre mots
- Rôle -> Une identité sans mot de passe, que l'on endosse
- Politique -> Un JSON : quelles actions sur quelles ressources
- Profil d'instance -> Le porte-badge du rôle sur l'instance
- Dans le lab -> Créer un rôle est refusé