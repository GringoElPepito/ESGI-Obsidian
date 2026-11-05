Soutenance au mois de Juillet.

Coordinateur du projet : 
- Kamal HENNOU
- ?
- ?

Projet AeroNex -> Equipementier d'aéronautique Européen 3 site 1 à Paris, 1 à Toulouse et 1 à Munich.

Modernisation de l'infrastructure :
- Sécurisation hybride
- Automatisation au niveau industrielle

Rompre les Silos

Convergence M1/M2 ->
Posture Consultant

Maquette Réseau et Infra virtu connectées.

CCTP formel + Justification TCO (Total Cost of Ownership) sur 3 ans fac à un DSI.

Pas de ClickOps

Paris -> 450 collaborateurs, Gouvernance, RH et DSI
Toulouse -> 500 collaborateurs, Usine d'Assemblage, environnement mixte IT:OT, automates industriels critiques et logistique SCADA.
Munich -> 250 collaborateurs R&D Aérodynamique, Grappes de calcul (HPC), très forte volumétrie de données sensibles.
150 collaborateurs Nomades

Total : 1200 collaborateurs.

ISO-27001
NIS2


Infra actuelle :
- Réseau -> Lien MPLS rigides/saturés, Wi-Fi WPA2 partagé
- Systèmes et identité -> Forêt AD 2012 obsolète, VM disparates sans durciseement.
- Processus -> Déploiements manuels, absence d'idempotence et de traçabilité?
- Sauvegardes & PRA -> Sauvegardes locales vulnérables aux ransomwares
- Sécurité -> Réseau plat, pas de MFA, zone OT perméable, aucun SIEM

Infra cible :
- Réseau -> SD-WAN dynamique (BGP), 802.1X/RADIUS, segmentation Campus avec vlan dynamique, intégration de la QoS, HA des équipements réseaux, STP, Double stack IPv4/IPv6 sur le coeur de réseau.
- Systèmes & identité -> Annuaire hybride (AD DS + Entra ID), standards CIS Benchmark
- Processus -> Flux 100% GitOps, automatisation Terraform/Ansible
- Sauvegardes -> Stockage S3 immuable (WORM) règle 3-2-1-1-0
- Zero Trust (ZTNA), NGFW, isolation OT, SIEM centralisé.