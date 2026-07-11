Pour une entreprise traitant des données sensibles, disposer d'une autorité de certification (CA) locale est un élément essentiel de la sécurité de son infrastructure informatique. Une CA interne permet de générer et de gérer des certificats numériques sans dépendre d'un tiers, garantissant ainsi un contrôle total sur les identités, les communications chiffrées et les accès aux ressources de l'entreprise. Cette maîtrise réduit les risques liés à la compromission des échanges, facilite l'authentification des utilisateurs et des équipements, et contribue au respect des exigences de conformité et de confidentialité. En centralisant la gestion des certificats, l'entreprise renforce également sa capacité à révoquer rapidement des accès en cas d'incident et à maintenir un niveau de sécurité élevé sur l'ensemble de son système d'information.

Pour garantir un certain niveau de sécurité, nous avons opté pour une architecture par délégation. En effet nous avons mis en place une Autorité de certification racine ainsi qu'une Autorité de certification délégué. L'autorité racine émet un certificat de délégation qui permet à l'autorité délégué d'émettre des certificats soumis à certaines contraintes permettant de limiter l'impact potentiel d'une compromission de l'autorité délégué. 
De cette manière, il nous est possible d'éteindre l'autorité racine dans le but de rendre celle-ci inaccessible et limiter le risque de compromission de la clé privé. Pour nous assuré que l'autorité racine reste bien éteinte, une supervision remonte une alerte lorsque l'instance répond au ping.

Voici les caractéristiques de l'instance root-ca.cenexis.lan :
- CPU : 1 core
- RAM : 512 Mb
- Stockage : 16G
- Instance : LXC
- OS : Rocky Linux 10.2
- état HA attendu : stopped

Voici les caractéristiques de l'instance sub-ca.cenexis.lan :
- CPU : 1 core
- RAM : 512 Mb
- Stockage : 16G
- Instance : LXC
- OS : Rocky Linux 10.2
- état HA attendu : stopped

Nous avons mis en place 2 chaînes de certification distinctes, la première est une chaîne de certification classique, cependant la seconde est une chaîne de certification Post-Quantique. Cette seconde chaîne nous permet de préparer notre infrastructure aux futures attaques qui seront rendus possible avec la démocratisation de l'informatique quantique. Malheureusement à ce jour, la plupart des services et protocoles ne supportent pas encore les algorithmes liés au chiffrement Post-Quantique, c'est la raison pour laquelle nous maintenons 2 chaînes de certification. De cette manière nous pouvons fournir un chiffrement adapté à tous nos services et faire la migration vers le chiffrement Post-Quantique si les mis à jours de nos services ajoutent le support de celui-ci.

Le déploiement et la configuration des autorités de certification racine et délégué sont entièrement automatisé via des playbook Ansible tout comme la génération de certificat client et serveur permettant de faciliter la configuration de nos différents services.

`root-ca.conf` :
```INI
[default]
name                    = root-ca
domain_suffix           = cenexis.com
aia_url                 = http://$name.$domain_suffix/$name.crt
crl_url                 = http://$name.$domain_suffix/$name.crl
ocsp_url                = http://ocsp.$name.$domain_suffix:9080
default_ca              = ca_default
name_opt                = utf8,esc_ctrl,multiline,lname,align

[ca_dn]
countryName             = FR
stateOrProvinceName     = Ile-de-France
organizationName        = CENEXIS
emailAddress            = admin@cenexis.com
organizationalUnitName  = Community
commonName              = "Root Ca"

[ca_default]
home                    = .
database                = $home/db/index
serial                  = $home/db/serial
crlnumber               = $home/db/crlnumber
certificate             = $home/$name.crt
private_key             = $home/private/$name.key
RANDFILE                = $home/private/random
new_certs_dir           = $home/certs
unique_subject          = no
copy_extensions         = none
default_days            = 3650
default_crl_days        = 365
default_md              = sha256
policy                  = policy_c_o_match

[policy_c_o_match]
countryName             = match
stateOrProvinceName     = optional
organizationName        = match
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional

[req]
default_bits            = 4096
encrypt_key             = yes
default_md              = sha256
utf8                    = yes
string_mask             = utf8only
prompt                  = no
distinguished_name      = ca_dn
req_extensions          = ca_ext

[ca_ext]
basicConstraints        = critical,CA:true
keyUsage                = critical,keyCertSign,cRLSign
subjectKeyIdentifier    = hash

[sub_ca_ext]
authorityInfoAccess     = @issuer_info
authorityKeyIdentifier  = keyid:always
basicConstraints        = critical,CA:true,pathlen:0
crlDistributionPoints   = @crl_info
extendedKeyUsage        = clientAuth,serverAuth
keyUsage                = critical,keyCertSign,cRLSign
nameConstraints         = @name_constraints
subjectKeyIdentifier    = hash

[crl_info]
URI.0                   = $crl_url

[issuer_info]
caIssuers;URI.0         = $aia_url
OCSP;URI.0              = $ocsp_url

[name_constraints]
permitted;DNS.0=.cenexis.com

[ocsp_ext]
authorityKeyIdentifier  = keyid:always
basicConstraints        = critical,CA:false
extendedKeyUsage        = OCSPSigning
keyUsage                = critical,digitalSignature
subjectKeyIdentifier    = hash
```