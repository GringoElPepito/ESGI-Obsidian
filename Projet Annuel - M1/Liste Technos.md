# Prio
## Virtu
- [ ] Virutalisation - Proxmox

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
- [ ] DNS - Windows ou Linux (ADGuard-Home)

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