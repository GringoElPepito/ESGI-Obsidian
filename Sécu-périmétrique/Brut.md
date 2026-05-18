# Intro
Sécurité périmétrique ensemble des mécanismes et stratégies visant à protéger les frontières d'un réseau informatique contre les intrusions, attaques et accès non autorisés.
Première ligne de défense d'une architecture de sécurité
Multi-couches

Périmètre réseau: Zone frontière séparant un réseau interne (de confiance) d'un réseau externe (non fiable comme Internet)
Objectif: Contrôler et filtrer le trafic entrant et sortant, détecter les menaces et réduire la surface d'attaque
Principé clé: "Trust but verify" - autoriser uniquement ce qui est explicitement nécessaire, tout le reste est bloqué

Pare-feu
Rôle : filtrer les paquets selon des règles prédéfinies
Type de firewall :
- Pare-feu statique (packet filtering) : contrôle basique sur adresses IP, ports, protocoles
- Pare-feu dynamique/Stateful inspection : garde l'état des connexions pour décider si un paquet est légitime
- Next Generation Firewall (NGFW) : inclut des fonctions avancées (filtrage applicatif, détection, d'intrusions, VPN, antivirus) - Ne pas forcément mettre en première ligne face à internet car moins adapté

IDS (Intrusion Detection System) : détecte des activités suspectes (signature, anomalies)
IPS (Intrusion Prevention System) : agit en temps réel pour bloquer l'attaque
Limite : peuvent générer des faux positifs/faux négatifs, nécessitent un ajustement constant

DMZ (Demilitarized Zones)
- Zone intermédiaire entre le réseau interne et Internet
- On y place les serveur accessibles publiquement (Web, mail, DNS)
Avantage :
- Si une machine en DMZ est compromise, l'attaquant

Les VPN (Virtual Private Network)
But : assurer un communication sécurisé de bout en bout via Internet
Mécanismes : chiffrement (IPSec, SSL), authentification forte
Exemple : Un employé distant se connecte au réseau de l'entreprise via un tunnel sécurisé

Les proxies et reverse-proxies
Proxy : agit comme intermédiaire entre l'utilisateur et internet, filtrant et journalisant les requêtes
Reverse proxy : protège les serveurs

Filtrage DNS et Web
- Empêche l'accès à des sites malveillants ou non autorisés
- Complément essentiel aux pare-feu et proxies

Types d'architecture :
- Architecture en Bastion : Une machine fortement sécurisée set de point d'entrée unique
- Architecture en couches d'oignon : superposition de plusieurs mécanismes (pare-feu, IDS, proxy, authentification multifactorielle), défense en profondeur
- Architecture Zero Trust : Remise en question du périmètre classique, ne jamais faire confiance, même à l'intérieur du réseau

Contrôle d'accès basé sur l'identité, le contexte et la conformité des appareils

Types de menaces :
- Bypass des pare-feu : via protocoles tunneling, ports non filtrés
- Attaques DDoS : Saturent le périmètre pour empêcher le fonctionnement normal
- Shadow IT : applications ou équipements non autorisés échappant au périmètre
- Mobilité et Cloud : le périmètre devient flou, obligeant à combiner périmétrique et sécurité applicative

Bonnes pratique
- Mettre en place une politique de sécurité stricte "Deny By default"
- Segmentation du réseau (VLAN, microsegmentation)
- Surveillance et journalisation du trafic (SIEM : Security Information and Event Manager)
- Tests réguliers (pentests, red team)
- Mise à jour continue des règles et signatures
- Sensibilisation des utilisateurs (phising, BYOD - Bring You Own Device)

Conclusion 
la sécurité périmétrique reste indispensable mais n'est plus suffisante
Elle doit être intégrée dans une stratégie globale de défense en profondeur, complété par des contrôles au niveau des applications, des données et des identités
Le futur s'oriente vers le Zero Trust et la sécurité cloud-centric, mais comprendre les bases périmtriques est fondamental pour tout ingénieur en réseaux et sécurité

# Active Directory
Composant central dans l'infrastructure Microsoft : annuaire des utilisateurs, groupes, contrôleurs de domaine, authentification, etc.

Si un attaquant compromet AD, cela lui donne un contrôle massif sur tout le SI (exfiltration, sabotage, persistance)

l'ANSSI observe que beaucoup d'attaques réussissent par mauavises pratiques d'administration, par défaut mal configurés ou par cloisonnement insuffisant

L'objectif du durcissement est de réduire les chemins d'attaque, limiter l'exposition des objets sensibles, segmenter les fonctions critiques et éviter les escalades de privilèges non contrôlées.

## Modèle de tiers/cloisonnement (Tiering)
- Tier 0 : coeur de confiance - les ressources les plus critiques (contrôleurs de domaine, comptes de service AD, comptes d'administration AD)
- Tier 1 : services métiers, serveurs applicatifs ou de données ayant une valeur métier élevée, mais moins sensibles que le coeur AD
- Tier 2 : postes de travail, endpoints, ressources moins sensibles, etc.

Une ressource d'un Tier inférieur ne doit pas pouvoir compromettre un Tier supérieur via des chemins d'attaque directs
on doit analyser et bloquer les chemins d'attaque entre tiers
on applique des politiques de sécurité plus strictes pour le Tier 0, avec un nombre restreint d'interactions autorisées
le cloisonnement ne se limite pas au réseau : il doit être logique, matériel, système et organisationnel
Ce cloisonnement est un processus itératif : on commence


Pour durcir AD, il faut savoir par où un attaquant pourrait progresser de façon latérale ou verticale.
L'analyse dse chemins d'attaque est donc centrale
Types de chemins :
- Relations de contrôle AD (transitivité)