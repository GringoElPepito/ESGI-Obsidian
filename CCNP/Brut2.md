Routeur 1 réseau = 1 interface


Les multis layer switch permettent de réaliser du routage intervlan.
MLS 1 réseau = X interface et 1 interface = X réseau

Les MLS ajoute un temps de latence quasiment nulle.


FIB Fowarding Information Base

Couche ACCESS, coût les plus faibles en fonction des besoins, cette couche vise à disparaître pour être jointe à la couche DISTRIBUTION (Architecture Collapsed).

pour convertir un port de niveau 2 en niveau 3 `no switchport` -> puis configuration du port



IPSec Remote Access à prioriser par rapport au VPN SSL (TLS)
VPN SSL était utiliser car peut fonctionner sur le port 443 permettant de contourner la censure. Cependant VPN SSL dépendant de trop de couches donc retour vers IPSec Remote Access

IPSec
Créer un tunnel sécurisé entre 2 LAN privés (L2L ou du site-to-site) ou entre un ordinateur itinérant vers les ressources de l'entrepris : VPN Remote-access

Phase 1 : IKE (Internet Key Exchange) sous protocole :
- ISAKMP (Internet Security Association Key Management Protocol)
But : Sécurisation de l'identification des futurs pairs IPSec - valide