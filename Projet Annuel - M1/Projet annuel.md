# Obligatoire
- Virtu -> Type 1 en Cluster
- Conteneurisation
- Stockage -> Distribué si possible Hyperconvergé
- Sauvegarde -> Hors site
- Automatisation
- Supervsion/Métrologie -> Alertes mails obligatoires
- Sécurité -> 2 niveaux de firewalls IDS/IPS option bastion
- Réseau -> GNS3 ou EVE-NG ou Pnet-LAB
- Systèmes -> AD, DNS, relais SMTP, messagerie légère etc...
- Cloud -> AWS, Azure, GCP au moins 1 service déployé
- PRA/PCA -> diagramme + liste des services critiques + procédures pas à pas
# Choix des technologies
## Système
- Virtualisation -> Proxmox
- Conteneurisation -> LXC & Kubernetes ou OpenShift
- Stockage : Ceph et si pas possible TrueNAS
- Sauvegarde : VEEAM
- Automatisation : Ansible
- Supervision/métrologie : Zabbix, Prometheus, Grafana
- Sécurité 
	- IDS : Snort
	- IPS : Suricata
	- VPN Point-to-Site : WireGuard
	- SIEM : Wazuh
- Système
	- AD : Windows Active Directory ou OpenLDAP
	- DHCP : ISC-KEA ou DHCP Windows
	- DNS : ADGuard Home ou DNS Windows
	- Mail 
		- Soit Mailcow
		- Soit Dovecot, Postfix

## Réseau
- Firewall externe : OPNsense ou Juniper