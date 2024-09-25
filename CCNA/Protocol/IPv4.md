### IPv4
les adresses IP sont séparés en classes, 32 bits réparties en 4 octets notation en décimal pointés. Il y a 2 parties dans l'adresse IP : Réseau / Host, on distingue ces 2 parties à l'aide du masque -> bit à 1 = Réseau / bit à 0 = Host.

#### Classe A
Pour les très grands réseaux.
Par défaut : 
- Réseau : 1er octet
- Host : 3 derniers octets
Masque : 255.0.0.0 -> /8 bitcount -> on compte le nombre de 1 dans le masque / CIDR = Classless InterDomain Routing.
Le 1er octet est compris entre 1 et 126
Il existe 126 réseaux de classe A chacun comporte 2²⁴ - 2 = 16 777 214 ID
on retire 2 adresses :
- La première : adresse réseau
- La dernière : adresse broadcast
#### Classe B
Pour les réseaux de tailles moyennes
Par défaut : 
- Réseau : 2 premiers octet
- Host : 2 derniers octets
Masque : 255.255.0.0 -> /16
Le 1er octet est compris entre 1 entre 128 et 191
Le second octet entre 0 et 255
Il existe 64 x 256 = 16384 réseaux de classe B
Chacun compte 2¹⁶ - 2 = 65535 IP

#### Classe C
Pour les réseaux de petites tailles
Par défaut : 
- Réseau : 3 premiers octet
- Host : le dernier octets
Masque : 255.255.255.0 -> /24
Le 1er octet est compris entre 1 entre 192 et 223
Le second octet entre 0 et 255
Il existe 64 x 256 x 256 = 2 097 152 réseaux de classe C
Chacun compte 2⁸ - 2 = 254 IP

#### Classe D
Pour le multicast (IP pour envoyer des flux à un groupe d'abonnés)
224.0.0.1 à 239.255.255.255

#### Classe E
Réservé (non utilisable)
240.0.0.0 à 255.255.255.254

#### IP localhost
127.X.Y.Z
ping localhost -> pile TCP/IP est fonctionnelle d'un point de vue protocolaire

Le [[Subnetting]] permet de revoir le fonctionnement des classes.