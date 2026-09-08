Pré-requis :
- ISO TALOS
- talosctl (outil cli)

# Etape 1
Installer 4 VM Talos :
- master01
	- CPU : 2
	- RAM : 2Gb
	- Disk : 20Gb
	- Network : Bridge
- worker01
	- CPU : 2
	- RAM : 2Gb
	- Disk : 20Gb
	- Network : Bridge
- worker02
	- CPU : 2
	- RAM : 2Gb
	- Disk : 20Gb
	- Network : Bridge
- worker03
	- CPU : 2
	- RAM : 2Gb
	- Disk : 20Gb
	- Network : Bridge

Configurer le réseau des instances Talos 

# Etape 2
Configuration du cluster Talos
Dans un shell taper les commandes suivantes :
```bash
export MASTER_IP=172.16.0.100 # Variable contenant l'IP du node Msa
export WORKER_IP=("172.16.0.101" "172.16.0.102" "172.16.0.103")
```
