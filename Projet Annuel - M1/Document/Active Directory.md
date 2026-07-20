L'Active Directory constitue le cœur de l'infrastructure informatique d'une organisation, car il centralise l'authentification, la gestion des utilisateurs, des groupes et des ressources. Dans une infrastructure critique, sa sécurisation est essentielle afin de garantir la confidentialité, l'intégrité et la disponibilité des systèmes d'information. Une compromission de l'Active Directory peut permettre à un attaquant d'obtenir des privilèges élevés, de se déplacer latéralement sur le réseau et de prendre le contrôle de l'ensemble du domaine. La mise en œuvre de mécanismes de sécurité, tels que le chiffrement des communications via LDAPS, contribue à protéger les échanges d'informations sensibles et à renforcer la résilience globale de l'infrastructure face aux cybermenaces.

Voici les caractéristiques de l'instance fty-lwads01.cenexis.lan :

| Paramètres      | Valeur                      |
| --------------- | --------------------------- |
| Type d'instance | VM                          |
| OS              | Windows Server 2022         |
| CPU             | 4 vCPU                      |
| RAM             | 4G                          |
| Stockage        | 60G                         |
| Interfaces      | eth0: VLAN112 -> 10.11.2.10 |

Voici les caractéristiques de l'instance fty-lwads02.cenexis.lan :

| Paramètres      | Valeur                      |
| --------------- | --------------------------- |
| Type d'instance | VM                          |
| OS              | Windows Server 2022         |
| CPU             | 4 vCPU                      |
| RAM             | 4G                          |
| Stockage        | 60G                         |
| Interfaces      | eth0: VLAN112 -> 10.11.2.11 |

Voici les caractéristiques de l'instance fty-lwads01.cenexis.lan :

| Paramètres      | Valeur                      |
| --------------- | --------------------------- |
| Type d'instance | VM                          |
| OS              | Windows Server 2022         |
| CPU             | 4 vCPU                      |
| RAM             | 4G                          |
| Stockage        | 60G                         |
| Interfaces      | eth0: VLAN112 -> 10.11.2.12 |
# Installation
Cette section portera sur l'installation de l'instance fty-wads03.cenexis.lan, nous n'aborderons pas l'installation du système en lui-même.
## Active Directory RODC
La première étape est de définir la configuration IP, nous définissons ici l'AD primaire (10.11.2.10 - fty-wads01.cenexis.lan) comme DNS principal, car étant la seule instance qui n'est pas en Lecture Seul, il est la machine qui sera forcément le plus à jour. De plus celui-ci n'étant pas exposé il traite bien moins de requête que les 2 instances RODC.
{ screen - conf IP }

Nous renommons par la même occasion l'interface pour facilement pouvoir identifier la configuration de celle-ci. Cette action est principalement une anticipation d'une potentiel évolution où les Active Directory auraient plusieurs interfaces. En les nommant de cette manière, il sera possible de différencier chaque interface sans avoir à entrer dans la configuration de chacune.
{ screen -interface }

Suite à cela, nous renommons la machine fty-wads03 et l'ajoutons au domaine cenexis.lan
{ screen - renommage }
{ screen - Ajout au domaine }

Nous redémarrons la machine pour prendre en compte les modifications précédentes. Puis nous procédons à l'installation du service Active Directory.
{ screen - installe AD }

Une fois installé, nous configurons l'Active Directory. Dans un premier temps on définit le domaine auquel lié l'Active Directory et le compte utiliser pour cela.
{ screen - CONFIG AD 1 }

Nous configurons ensuite les capacités de l'Active Directory, nous activons le DNS, le Catalogue Global ainsi que l'option RODC. Cette dernière option fais en sorte qu'aucune modification sur l'Active Directory ne soit réalisable depuis cette machine. 
{ screen - CONFIG AD 2 }

Nous validons le reste de la configuration et lançons l'installation finale. 
{ screen - Installe POST CONFIG AD }

A la fin de cette installation, la machine redémarre et une fois rallumé nous pouvons voir que la machine a bien été ajouté au contrôleur de domaine en Lecture seule
{ screen - DOMAIN CONTROLLERS}

## Mise en place LDAPS
Récupérer la chaîne de certification :
```powershell
scp desigual@10.11.2.21:/home/desigual/sub-ca/ca-chain.crt ./
```

Importation du certificat dans le magasin Windows :
```powershell
Import-Certificate -FilePath .\ca-chain.crt  -CertStoreLocation 'Cert:\LocalMachine\Root' -Verbose
```

Création d'un fichier `request.inf` :
```inf
[Version]
Signature="$Windows NT$"

[NewRequest]
Subject = "CN=fty-wads03.cenexis.lan, OU=Community, O=CENEXIS, L=Fontenay-sous-Bois, S=Ile-de-France, C=FR"
MachineKeySet = TRUE
KeyLength = 4096
KeySpec = 1
KeyUsage = 0xA0
Exportable = TRUE
SMIME = FALSE
RequestType = PKCS10
HashAlgorithm = SHA256
ProviderName = "Microsoft Software Key Storage Provider"
ProviderType = 0

[Extensions]
2.5.29.17 = "{text}"
_continue_ = "dns=fty-wads03.cenexis.lan"
```

Création d'une demande de signature :
```powershell
certreq -new request.inf fty-wads03.csr
```

Copie du fichier de demande de signature sur l'autorité de certification intermédiaire :
```powershell
scp ./fty-wads03.csr desigual@10.11.2.21:/home/desigual/sub-ca/req/
```

Sur l'autorité de certification intermédiaire :
```bash
cd /home/desigual/sub-ca
sudo openssl ca -batch -config sub-ca.conf -subj "/CN=fty-wads03.cenexis.lan/OU=Community/O=CENEXIS/L=Fontenay-sous-Bois/S=Ile-de-France/C=FR" -create_serial -in req/fty-wads03.csr -out certs/fty-wads03.crt -extensions server_ext -days 365 -passin pass:<pswd_private_key_sub_ca>
sudo openssl crl2pkcs7 -nocrl -certfile certs/fty-wads03.crt -certfile sub-ca.crt -certfile root-ca.crt -out certs/fty-wads03.p7b
sudo cp certs/fty-wads03.p7b ../
sudo chown desigual:desigual ../fty-wads03.p7b
```

Récupération du certificat sur l'AD :
```powershell
scp desigual@10.11.2.21:/home/desigual/fty-wads03.p7b ./
```

Installation du certificat :
```powershell
certreq -accept .\fty-wads03.p7b
```

Vérification du LDAPS :
```powerhsell
netstat -ano | findstr :636
```

# Exploitation

## Accès

Concernant les accès, il n'y en a qu'un seul qui est celui fournit par le bastion (fty-lbst01.cenexis.lan). Ce dernier fournit un accès RDP permettant un accès direct à la machine ce qui est parfait pour l'administration des machines ou du domaine. L'intégralité des opérations ciblant l'Active Directory doivent être réalisé au travers du bastion.

