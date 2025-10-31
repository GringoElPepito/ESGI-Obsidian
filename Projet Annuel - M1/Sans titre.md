# Partie prenante
- Notre equipe
- Client -> Cenexi
- Fournisseurs ->
	- Proxmox
	- Hetzner
	- Dell
	- Microsoft
	- Kubernetes
	- TrueNAS
	- OPNsense
	- Prometheus
	- Grafana


# Objectifs
- Création d'une infrastructure informatique supportant la production de l'entreprise Cenexi, L'infrastructure doit être
	- Utiles, l'infrastructure doit fournir l'ensemble des outils et services permettant de conduire et suivre la production en temps réel
	- Sécurisé, penser pour garantir la confidentialité et l'intégrité des données liés à l'activité de l'entreprise
	- Redondé, l'infrastructure ne doit pas cesser de fonctionner même en cas de panne et ne pas impacter la production de l'entreprise
	- Tracé, l'infrastructures doit garder toutes les données relatives et la production et garantir la disponibilité de ces données même en cas d'incident majeur

# Livrables
Livrables projets
- Systèmes de virtualisation
- Service de gestion des utilisateurs
- Solution de sauvegarde
- Solutions de journalisation
- Solutions de supervision
- Documentation technique
	- DAT
	- LDD
	- cahier des charges
- Documentation 
	- Facture
	- Devis
	- GANTT
	- 


# RACI

|                                     | Julien | Soratha | Vivien | Miguel |
| ----------------------------------- | ------ | ------- | ------ | ------ |
| Proxmox                             | C      | A       |        | R      |
| Clusterisation Proxmox              | C      | A       |        | R      |
| Kubernetes                          | R      |         |        | A      |
| TrueNAS                             |        | A       | R      |        |
| Proxmox Backup                      | A      | R       |        | C      |
| Windows AD                          | A      | R       | C      |        |
| Mattermost                          | R      |         | A      |        |
| Mailcow                             |        |         | A      | R      |
| FreePBX                             |        |         | R      |        |
| NextCloud                           |        | R       | A      |        |
| Reposync                            |        | R       | A      |        |
| Harbor                              |        |         |        | R      |
| Pare-feu                            | R      |         | A      |        |
| Serveur PKI                         | A      | I       | R      | I      |
| Serveur PKI Issuer                  | A      | I       | R      | I      |
| Prometheus                          | C      | R       | A      |        |
| Grafana                             | R      | C       | A      |        |
| RSYSLOG                             | C      | A       | R      |        |
| Wazuh                               | A      | C       |        | R      |
| Conception de l'architecture Réseau | C      |         |        | R      |
| Plan d'adressage                    | C      |         |        | R      |
| Configuration du switching          | R      |         |        | A      |
| Configuration du routage            | R      |         |        | A      |
| Configuration des pare-feu          | A      | R       |        | C      |
| Configuration du VPN Point-to-site  | R      | A       |        | C      |
| Configuration du VPN site-to-site   |        | A       | R      | C      |
| Configuration de l'IDS/IPS          | A      | R       |        |        |
| Automatisation via Ansible          | A      |         |        | R      |
