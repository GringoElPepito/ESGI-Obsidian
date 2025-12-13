# Infra
Nom de domaine :
- desigual.lan -> AD
- ludovik.lan

Réseau :
- LAN : 172.16.0.0/24 -> Passerelle : 172.16.0.254
- SAN : 10.99.99.0/29
- vMotion : 10.100.100.0/29
- Fault Tolerance : 10.101.101.0/29

Machine :
- TrueNAS -> san.ludovik.lan
	- LAN : 172.16.0.100
	- SAN : 10.99.99.5
- ESXi 1 -> esxi1.ludovik.lan
	- LAN : 172.16.0.11
	- SAN :
		- 10.99.99.1
		- 10.99.99.2
	- vMotion : 10.100.100.1
	- Fault Tolerance : 10.101.101.1
- ESXi 2 -> esxi2.ludovik.lan
	- LAN : 172.16.0.12
	- SAN :
		- 10.99.99.3
		- 10.99.99.4
	- vMotion : 10.100.100.2
	- Fault Tolerance : 10.101.101.2
- OPNSense -> srv.ludovik.lan
	- LAN : 172.16.0.254
- vCenter -> vcenter.ludovik.lan
	- 

Customisation de l'OS :
- Linux :
	- Installation des VMWare tools `open-vm-tools`
	- Installation de PERL
	- Autorisation de lancement de script via `open-vm-tools` avec la commande `vmware-toolbox-cmd config set deployPkg enable-custom-scripts true`
- Windows :
	- Installation des VMWare tools

