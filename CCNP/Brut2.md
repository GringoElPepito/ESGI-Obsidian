Routeur 1 réseau = 1 interface


Les multis layer switch permettent de réaliser du routage intervlan.
MLS 1 réseau = X interface et 1 interface = X réseau

Les MLS ajoute un temps de latence quasiment nulle.

Focus NTP en v4 si possible

FIB Fowarding Information Base

Couche ACCESS, coût les plus faibles en fonction des besoins, cette couche vise à disparaître pour être jointe à la couche DISTRIBUTION (Architecture Collapsed).

pour convertir un port de niveau 2 en niveau 3 `no switchport` -> puis configuration du port



IPSec Remote Access à prioriser par rapport au VPN SSL (TLS)
VPN SSL était utiliser car peut fonctionner sur le port 443 permettant de contourner la censure. Cependant VPN SSL dépendant de trop de couches donc retour vers IPSec Remote Access

IPSec
Créer un tunnel sécurisé entre 2 LAN privés (L2L ou du site-to-site) ou entre un ordinateur itinérant vers les ressources de l'entrepris : VPN Remote-access

Phase 1 : IKE (Internet Key Exchange) sous protocole :
- ISAKMP (Internet Security Association Key Management Protocol)
But : Sécurisation de l'identification des futurs pairs IPSec 
Les 2 paires IPSec vont négocier les 2 paires une politique communes :
-  Méthode d'authentification : PSK (pre-shared key), RSA-SIG, RSA-encrypted-nonce
- Algo de chiffrement de la suite de la Phase 1 et de la négociation de la phase 2 : DES, 3DES, AES{128,192.256}
- Algo de hashage de la suite de la phase 1 et de la négociation de la phase 2 : MD5, SHA-1, SHA-256
- Choix du groupe DH (Diffie Hellman) -> Crypto-système asymétrique permettant aux 2 paires de trouver un secret commun sans jamais l'échanger en clair (group 14 minimum si possible 19 ou 20)
- Lifetime : 86400s (24H) par défaut
A la fin de la phase 1 : Une SA (Security Association) bidirectionnelle

Phase 2 : IPSec
But : sécurisation des données de LAN à LAN
Les 2 paires négocient une politique identique :
- Protocole d'encapsulation : AH (Pas de chiffrement possible), ESP (Encapsulation Security Payload)
- Méthode d'encapsulation : tunnel (mode par défaut : création de nouvelles en-têtes IP) / Transport (pas de nouveaux header IP)
- Algo de chiffrement de l'échange des données de LAN à LAN : DES, 3DES, AES
- Algo de hashage de l'échange des données de LAN à LAN : MD5, SHA
- Optionnellement : Activer PFS (DH à la phase 2) : Chaque session à sa propre clé de chiffrement
- Lifetime : 3600s (1H) ou 4GB d'info échangées
A la fin de la phase 2 : Deux SA unidirectionnel

On utilisera le mode transport si les 2 paires sont capables de communiquer sans IPSec.

Pour les choix d'algo de chiffrement, il ne faut pas forcément prendre le plus élevé car plus gros coût sur les performances.

Configuration Phase 1 :
```bash
crypto isakmp policy 1
authenti pre
encryp des
hash
md5
group 2
lifetime 43200
exit
/// Définition de la PSK
crypto isakmp key toto1234 address 2.2.2.1
/// Config Phase 2
crypto ipsec transform-set TS_VPN esp-des esp-md5
exit
/// Créer une crypto ACL -> ACL qui définit le trafic qui doit passer dans le tunnel IPSec
ip access-list stand ACL_VPN
permit 192.168.0.0 0.0.255.255
exit
/// Créer une crypto map pour recoller les éléments configurés précédemment crypto map CM_VPN
crypto map CM_VPN 10 ipsec-isakmp
match address ACL_VPN
set transfor TS_VPN
set peer

int e0/2
crypto map CM_VPN

```

Pour que des routeurs échanges des tables de routage, il faut que ces derniers partagent un réseau en commun.


L'instruction network dans les protocoles de routages permet au routeur de vérifier quelle(s) interface(s) appartient au réseau renseigné et va activer OSPF sur les interfaces concernées et ajouter le réseau à la liste des réseaux à distribués.

IPSec ne supporte pas le multicast et le broadcast, c'est pour cela que l'on utilise GRE qui permet d'encapsulé le trafic