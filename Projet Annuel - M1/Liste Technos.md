# Prio
## Virtu & Conteneurisation
- [ ] Virutalisation - Proxmox
- [ ] Conteneurisation - Kubernetes
	- 1 Master
		- 4vCPU
		- 16Gb RAM
		- 

## Stockage
- [ ] TrueNAS

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
- [ ] Dashboard - Grafana
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