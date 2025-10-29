# Prio
## Virtu & Conteneurisation
- [ ] Virutalisation - Proxmox
- [ ] Conteneurisation - Kubernetes 
	- 1 Master -> Sur Proxmox
		- 2vCPU
		- 8Gb RAM
		- Disk
			- System -> 40Go
	- 2 Workers -> Sur Proxmox
		- 4vCPU
		- 16Gb RAM
		- Disk
			- System -> 100Go

## Stockage & Sauvegarde
- [ ] Stockage - TrueNAS
	- 4vCPU
	- 8Gb RAM
	- DIsk
		- System -> 32Go
		- Data -> Tout l'espace disque supplémentaire disponible
- [ ] Sauvegarde - VEEAM BACKUP
	- 2vCPU
	- 4Gb RAM
	- Disk
		- System -> 60Go
		- 

## Système et Com
- [ ] Annuaire - Windows Active Directory
	- 2 VM
		- 2vCPU
		- 2Gb RAM
		- Disk
			- System -> 60Go

- [ ] DHCP - Windows ou Linux (ISC-KEA)
	- Si Windows => sur VM Active Directory
	- Si Linux
		- 2 LXC
		- 1vCPU
		- 1Gb RAM
		- Disk
			- System -> 16Go

- [ ] DNS - Windows ou Linux (ADGuard-Home)
	- Si Windows => sur VM Active Directory
	- Si Linux
		- 2 LXC
		- 1vCPU
		- 1Gb RAM
		- Disk
			- System -> 16Go

- [ ] BDD - PostGreSQL
	- 3 LXC
	- 2vCPU
	- 4Gb TAM
	- Disk
		- System -> 16Go
		- Data -> 250Go
- [ ] Communication - Mattermost 
	- 1 LXC
	- 2vCPU
	- 1Gb RAM
	- Disk
		- System -> 32Go

## Securité
- [ ] Firewall - OPNsense
	- VM
	- 2vCPU
	- 2Gb RAM
	- Disk
		- System -> 32Go

- [ ] PKI CA - Linux
	- LXC
	- 1vCPU
	- 512Mb RAM
	- Disk
		- System -> 16Go

- [ ] PKI CA Issuer - Linux
	- LXC
	- 1vCPU
	- 1Gb RAM
	- Disk
		- System -> 32Go

## Supervision
- [ ] Collecteur - Prometheus
	- Si Kubernetes -> pas de requirements
	- SI LXC
		- 1vCPU
		- 2Gb RAM
		- Disk
			- System -> 16Go
- [ ] Dashboard - Grafana
	- Si Kubernetes -> pas de requirements
	- Si LXC
		- 1vCPU
		- 2Gb RAM
		- Disk
			- System -> 16Go
- [ ] SYSLOG - RSYSLOG
	- LXC
	- 1vCPU
	- 2Gb RAM
	- Disk
		- System -> 16Go
		- Data -> 100Go
- [ ] SIEM - Wazuh
	- LXC 
	- 2vCPU
	- 4Gb RAM
	- Disk
		- System -> 16 Go
		- Data -> 100Go

# Secondaire

## Sécurité
- [ ] SSO - KeyCloak