# Architecture des réseaux opérateurs
## Réseau d'accès
Réseau d'accès, est la partie du réseau qui connecte les utilisateurs finaux aux services de l'opérateur, on distingue plusieurs type de technologies d'accès :
- Fibre optique (FTTH, FTTB, FTTC) : haut débit + faible latence
- xDSL : 
- Technologies mobiles : 
- Satellites : 

## Réseau de transport
Réseau de transport connecte les infra d'accès aux centres de données et aux points d'échange il repose sur :
- MPLS (MULTIPROTOCOL LABEL SWITCHING) : Optimisation du routage des paquets
- Technologies optiques (DWDM, SDH) : Transmission à très haut débit
- IP/MPLS : Pour la gestion du trafic et la garantie de la qualité de service (QoS)

## Backbone
Le réseau de coeur assure l'acheminement des données entre les différents noeuds du réseau. Il en constitue de routeurs haute capacité et de liaisons à haut débit pour :
- La gestion des interconnexions entre réseau nationaux et internationaux
- tansit ip : Achat de connectivié auprès d'un fournisseur Tier
- IXP (Internet Exchange Point) : Plateformes permettant l'interconnexion entre différents réseaux 

## Protocoles et Techno utilisés
- BGP : Utilisé pour le routage entre opérateurs et la gestion des annonces préfixe IP
- OSPF et IS-IS : Protocoles de routage interne utilisés pour assurer la redondance et la résilience
- MPLS : Utilisé pour segmenter et acheminer efficacement le trafic 

## QoS et gestion du trafic
Les opérateurs mettent en place des mécanismes pour garantir une qualité de service optimale
- Diffserv (differentiated service) : priorisation des flux en fonction de la classe de service.
- Traffic engineering (TE) : optimisation des routes en fonction de la congestion
- Load balancing : répartition des chemin réseau

## Sécurité et Résilience
- Firewall et IDS/IPS : Protection contre les attaques et filtrage du trafic
- DDOS mitigation: Protection contre les attaques DDOS
- Redondance et HA : mis en place de liens de secours et de mécanismes de failover

## Services et infra supportés
Proposition de services :
- Internet fixe et mobile : fourniture de connectivité  aux particuliers
- Téléphonie IP et traditionnelle : Voix sur IP (VoIP) et services RTC
- Télévision sur IP (IPTV) : Distribution de contenu multimédia via IP

## Tendance et évolutions des réseaux opérateurs
Les technologies évoluent pour répondre aux nouveaux besoins des utilisateurs et aux contraintes techniques
- NFV (Network function virtualisation) : virtualisation des équipements réseau
- SDN (Software-defined networking) : Automatisation et gestion centralisée des réseaux
- ZERO-TOUCH PROVISIONING (ZTP) : Déploiement automatique des équipements