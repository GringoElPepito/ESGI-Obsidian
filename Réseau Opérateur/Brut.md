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

# OSPF
## Fonctionnement de OSPF
Protocole de routage dynamique, permettant de faire communiquer des routeurs au sein d'un même AS (Autonomous System).
Il repose sur l'algo de Dijkstra (SPF - Shortest Path First) pour calculer les chemins les plus courts vers une destination
Caractéristiques :
- Protocole à état de lien et non à vecteur de distance
- Convergence rapide grâce à la mis à jour incrémentales des routes
- Hiérarchisation du réseau en aires pour optimiser les performances
- support du VLSM (Variable Length Subnet Mask) et du CIDR (CLASSLESS INTER-DOMAIN Routing)
- Utilisation du multicast pour la mise à jour des routes (224.0.0.5 et 224.0.0.6)
Masque inversé car les 0 sont plus rapide à interprété

## Types de routeurs
- Routeur Interne : situé dans une seule aire OSPF
- Routeur de bordure d'aire (ABR - Area Border Router) : Connecte plusieurs aires OSPF
- Routeur autonome de bordure (ASBR - Autonomous System Boundary Router) : Connect OSPF à d'autres protocoles de routage (ex. BGP, EIGRP)

## Types de paquets :
- Hello : Etablit et maintient les relations de voisinage
- Database Description (DBD) : Synchronise les base de données OSPF
- Link-State request (LSR) : Demande les mis à jour de l'état de lien
- Link-state Update (LSU) : Contient les mis à jour des informations de routage
- Link-state ACKNOWLEDGMENT (LSACK) : Confirme la réception des mises à jour

## Types d'aires OSPF :
- Aire Backbone (Area 0) : L'aire principale à laquelle toutes les autres doivent être connecté
- Aires standards : contiennent l'ensemble des routes et des LSAS
- Aires stub : Réduisent la taille des tables de routage en filtrant les LSAS Externes
- Aires Totally stub : Filtrent à la fois LSAS externes et internes non nécessaires.
- Aires NSSA ( )

# VPN
## VPN IPsec
Phase 1 : négociation de la connexion entre les 2 terminaux pour la création du tunnel 
Phase 2 : Autorisation des VLANs distant à passé au travers du tunnel
Le routage dynamique fonctionne sur des transmission TCP ce qui par défaut rend impossible son fonctionnement à travers un tunnel IPsec.
IPSec (Internet Protocol Security) : FOURNIT des mécanismes de Chiffrement et d'authentification pour sécuriser les communications

## VPN GRE
GRE = Generic Routing Encapsulation : permet d'encapsuler différents protocoles dans un tunnel IP
IPSec over GRE permet d'utiliser des protocoles de routages