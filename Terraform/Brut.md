vérifier qu'un compte est bien connecté à AWS-CLI
```bash
aws sts get-caller-identity
```

Lorsqu'un fichier est en cours de modification sur un stockage S3, celui-ci est verrouillé.

AWS
Compte -> Un conteneur étanche de ressources et de factures identifié par douz chiffres.

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

Inputs -> variables.tf

