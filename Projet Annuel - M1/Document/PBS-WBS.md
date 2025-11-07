# Gestion Projet
- Gantt
- Kanban
- DTI (Document Technique d'Installation)

# Virtualisation et Systèmes
Proxmox
- Installation des nodes 
- Clusterisation des nodes
- Ajout du stockage
- Configuration des accès
- Déploiement des images de virtualisation LXC + VM
- Automatisation de l'installation et de la configuration

Kubernetes
- Installation du node master
- Installation des workers
- Configuration des accès
- Automatisation de l'installation et de la configuration

TrueNAS
- Installation du serveur
- Configuration des accès
- Création des volumes de stockages
- Automatisation de l'installation et de la configuration

Proxmox Backup Server
- Installation du serveur
- Configuration des accès
- Configuration de la sauvegarde
- Test de sauvegarde et de restauration
- Externalisation des sauvegardes
- Automatisation de l'installation et de la configuration

Windows Active Directory
- Installation des serveurs
- Ajouts des rôles
- Configuration des rôles
- Clusterisation des serveurs
- Automatisation de l'installation et de la configuration

Windows Active Directory de Gestion
- Installation du serveur
- Ajouts du rôle
- Enregistrement du serveur auprès de l'AD
- Automatisation de l'installation et de la configuration

DHCP -> à voir
DNS -> à voir
PostgreSQL 
- Installation des nodes
- Configuration des nodes
- Clusterisation du cluster
- Tests d'écritures/lectures
- Tests de haute disponibilité
- Automatisation de l'installation et de la configuration

Mattermost
- Installation du serveur
- Configuration du serveur
- Création de la base de données et des accès sur le cluster PostgreSQL
- Ajout de l'intégration à l'Active Directory
- Automatisation de l'installation et de la configuration

Mailcow
- Installation du serveur
- Configuration du serveur
- Ajout de l'intégration à l'Active Directory
- Exposition des services sur Internet
- Automatisation de l'installation et de la configuration

FreePBX
- Installation du serveur
- Configuration du serveur
- Automatisation de la création des utilisateurs à partir de l'annuaire Active Directory
- Exposition des services sur Internet
- Automatisation de l'installation et de la configuration

Repo Rocky Linux
- Installation du serveur
- Configuration du serveur
- Automatisation de l'installation et de la configuration

Harbor
- Installation du serveur
- Configuration du serveur
- Automatisation de l'installation et de la configuration

OPNsense - Virtu
- Installation du pare-feu
- Configuration des accès
- Configuration de base
- Définition des règles de pare-feu
- Configuration du VPN Point-to-site (OpenVPN)

PKI CA
- Installation du serveur
- Configuration du serveur
- Enregistrement des certificats
- Automatisation de l'installation et de la configuration
- Arrêt du serveur

PKI CA Issuer
- Installation du serveur
- Configuration du serveur
- Automatisation de l'installation et de la configuration

Prometheus
- Installation du serveur
- Configuration du serveur
- Configuration des exporteurs
- Automatisation de l'installation et de la configuration

Grafana
- Installation du serveur
- Configuration du serveur
- Connexion à Prometheus
- Automatisation de l'installation et de la configuration

RSYSLOG
- Installation du serveur
- Configuration du serveur
- Automatisation de l'installation et de la configuration

Wazuh
- Installation du serveur
- Configuration du serveur
- Récupération des logs depuis le serveur RSYSLOG
- Configuration des règles d'alertes
- Automatisation de l'installation et de la configuration

# Réseau
Architecture
- Schéma d'architecture
- Plan d'adressage
- Matrice de Flux

OPNsense OU Juniper -> FW Externe
- Installation des instances
- Configuration de base
- Clusterisation des pares-feux
- Configuration des règles de pare-feu
- Configuration du VPN site-to-site (IPsec + VxLAN)
- Automatisation de la configuration

Pfsense OU OPNsense si Juniper en externe -> FW Interne
- Installation des instances
- Configuration de base
- Clusterisation des pares-feux
- Configuration du routage LAN
- Configuration des règles de pare-feu
- Automatisation de la configuration

NetCore 
- Configuration des interfaces de gestion
- Configuration des VLANs
- Configuration du RPVST+
- Configuration des ports
  - VLANs
  - RPVST+
  - BPDU Guard
  - Port-Security
- Automatisation de la configuration

Distribution
- Configuration des interfaces de gestion
- Configuration des VLANs
- Configuration du RPVST+
- Configuration des ports
  - VLANs
  - RPVST+
  - BPDU Guard
  - Port-Security
- Automatisation de la configuration

Access
- Configuration des interfaces de gestion
- Configuration des VLANs
- Configuration du RPVST+
- Configuration des ports
  - VLANs
  - RPVST+
  - BPDU Guard
  - Port-Security
- Automatisation de la configuration
