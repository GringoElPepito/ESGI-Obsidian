# Infra
Nom de domaine :
- desigual.lan
- ludovik.lan

Réseau :
- LAN : 172.16.0.0/24
- SAN : 10.99.99.0/29
- vMotion : 10.100.100.0/29
- Fault Tolerance : 10.101.101.0/29

Customisation de l'OS :
- Linux :
	- Installation des VMWare tools `open-vm-tools`
	- Installation de PERL
	- Autorisation de lancement de script via `open-vm-tools` avec la commande `vmware-toolbox-cmd config set deployPkg enable-custom-scripts true`
- Windows :
	- Installation des VMWare tools

