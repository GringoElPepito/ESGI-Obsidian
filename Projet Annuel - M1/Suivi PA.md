# Points de blocage
- Réseau
	- SPAN 
		- BPDU Guard OK
		- PortFast
	- IPSec GRE
- Système
	- Kubernetes
		- Question sur la pertinence dans l'infra
		- Si pas maintenu fallback de GLPI et JumpServer  vers VM - Docker
	- Mailcow
		- VM - Docker
	- Wazuh
		- Création des règles d'alertes
- Cloud
	- On sait pas quoi mettre, hésite entre
		- Azure AD Cloud
		- Server Web pour data production

# ToDo
- Réseau
	- [ ] STP - test
	- [ ] IPSec GRE
	- [ ] Configuration Firewall Interne
	- [ ] Configuration Firewall Externe
- Système
	- [ ] Wazuh - Configuration Alerte
	- [ ] Rsyslog - Installation Agent Wazuh
	- [ ] Zabbix - Tout
	- [ ] Proxmox Backup Server - Tout
	- [ ] 
- Cloud



# Compte rendu séance suivi PA
Pour soutenance:
- Rajouter contexte existant et vendre le projet avec les problemes de l'entreprise et ce qu'on propose pour resoudre ces problèmes
- Faire une bonne demo pour scénarios avec scripts python déclenchant attaque et interfacer twilio paging etc
- Zabbix justificatifions de non utilisation de proxy (Chaque site se gère lui-même)

Pour le schéma d'architecture:

- Siem en haut et fleche sort du siem + ajouté serveur Rsyslog
- Zabbix en haut
- Mettre en place une vue hiérarchique et horizontal

Location Serveur + Cloud :
- Hetzner faire raid 1 pcq leur stockage pète souvent 
- Offre elastic scaleway, bare metal tarif à l'utilisation donc moins cher 
- Apply partie startup progamme sur aws pourr 1000€ de crédit 

Réseau :
- Bien régler mtu mss pour gre ipsec 

Infra :
- Remplacement de MailCow par Poste.io 
- Ajout Stockage cloud pour justification lien VPN intersite




# RETOUR PA 
Rendu 1 :
- Annexe pour les parties connaissances pure
- Méthode AGILE reformulé le texte
- Cloud a pousser pipeline avec schema