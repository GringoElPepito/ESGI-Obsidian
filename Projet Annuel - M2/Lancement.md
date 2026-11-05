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
- Systèmes & identité -> Annuaire hybride (AD DS + Entra ID),, IDP Cloud (OKTA, Entra ID), Keycloack/authentik SAML ou OCID, Socle Linux RHEL/ Rocky avec standards CIS Benchmark, gestion centralisé des postes clients avec GPO et MDM.
- Processus -> Flux 100% GitOps, automatisation Terraform/Ansible, IaC, CI/CD, ArgoCD
- Sauvegardes & PRA -> Stockage S3 immuable MinIO ou Ceph (WORM) règle 3-2-1-1-0
- Sécurité -> Zero Trust (ZTNA) pour accès distant, NGFW, isolation OT, SIEM centralisé, bastion (jumpserver), MFA obligatoire, IPS/IDS, EDR/SIEM/SOC, Prometheus/Grafana.
Hub and Spoke

RTO -> moins de 2H
RPO -> moins de 15min

Jalon 1 -> Fin janvier 2027 :
- Livrables :
	- Mémoire technique
	- matrice de conformité CCTP
	- chiffrage TCO sur 3 ans
	- incluant des métriques FinOps/Green IT
Pitch de 10 min (15% de la note finale)

Jalon 2 -> Fin avril 2027
- Livrables :
	- DAT complet et dépôt git validé
	- IaC
	- Revue : Audit de code Terraform/ansible sur machine (20%de la note)

Jalon 3 : Fin Juin 2027
- Recette 


35mins du grand orales, 15 minutes dédiées à l'injection de pannes en direct par le jury. Vos architectures cibles sont votre seul filet de sécurité

Moins de 5s pour reprise trafic BGP.
S3 Object Lock (WORM) restauration via RunBook
Tentative de suppression malveillante des backups.

10min de pres, 15min d'injection de panne, 10min de question/réponse.