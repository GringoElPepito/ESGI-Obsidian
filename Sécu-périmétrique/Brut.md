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
Limite : peuvent générer des faux positifs/faux négati