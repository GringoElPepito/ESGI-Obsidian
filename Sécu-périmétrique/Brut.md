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
- Relations de contrôle AD (transitivité) : Si A contrôle B et B contrôle C, la compromission de A peut permettre la compromission de C
- Exploitation de protocole AD / Windows légitimes (RPC, SMB, Kerberos, LDAP) dans un contexte détourné ou mal configuré
- Secrets d'authentification (mots de passe, hashes, clés), réutilisation de comptes locaux, serviescripteurs d'authentification, etc.
- Agents de gestion centralisée, services installés, scripts automatisés, etc.
- Chemin hors AD, via infrastructure physique, virtuelles ou cloud, pouvant rétroagir sur AD

Etapes d'analyse :
- identification initiale des ressources sensibles (Tier 0 en priorité)
- Cartographie des relations légitimes entre objets AD / systèmes
- Détection des liens transitifs ou implicites
- Evaluation de la difficulté d'exploitation de chaque chemin (coût, conditions, faisabilité)
- Priorisation des chemins à traiter (ceux plus exploitables)

Bonnes pratiques :
ANSSI propose plusieurs recommandations :
- Séparer les postes bureautiques et les postes d'administrations
- Ne pas utiliser un compte d'administration pour des usages courants
- Ne pas mutualiser les usages d'administration entre Tiers sans mesures de sécurité renforcées
- Comptes, postes et méthodes d'administration dédiés pour chaque zone de confiance (Tier)

Durcissement système / logiciel
Bloqué par défaut les MCP

Principe du moindre privilège est fondamental : donner uniquement les droits strictement nécessaires pour une tâche, rien de plus.
JEA en powershell

Journalisation et détection

# IDS/IPS

Bonne pratique pour optimisation IDS en détection et IPS en prévention uniquement en se basant sur les résultats de l'IDS

NIDS -> Network IDS
HIDS -> Host IDS

Méthodes de détection :
- Détection par signatures
	- Compare les paquets à une base de données d'attaques connues
	- Similaire à un antivirus
	- Avantage : rapide, peu de faux positifs
	- Inconvénient : inefficace contre les attaques inconnues (Zero-Day)
- Détection par anomalies
	- Crée un modèle de comportement "normal" du réseau
	- Détecte les écarts significatifs
	- Avantage : Détecte les attaques nouvelles
	- Inconvénient : Taux de faux positifs élevé, nécessite un apprentissage
	- En train d'exploser avec l'IA
- Détection comportementale / heuristique
	- Principe de l'analyse heuristique : repérer des comportements suspects et les bloqués, cependant génère beaucoup de faux positif
	- Analyse le comportement global d'un utilisateur, d'une machine ou d'un protocole

- Placement réseau
	- En ligne (inline) : trafic passe à travers l'IPS -> possibilité de bloquer
	- En écoute (passive) : IDS observe le trafic via un port miroir -> pas de blocage, uniquement détection
- Composants typiques
	- Capteurs : capturent les paquets réseau
	- Serveur d'analyse : exécute les règles de détection
	- Console de gestion : interface pour les alertes, configuration, statistiques
	- Base de signatures/ règles : patterns connus d'attaques