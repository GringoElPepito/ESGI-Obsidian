Pré-requis :
- ISO TALOS
- talosctl (outil cli)
- arkade (outil cli)

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
# Variable contenant l'IP du node Master
export MASTER_IP=172.16.0.100 
# Variable contenant les IP des nodes Worker
export WORKER_IP=("172.16.0.101" "172.16.0.102" "172.16.0.103") 

# Récupération des disques
talosctl get disks --insecure --nodes $MASTER_IP 

# Génération des fichiers de configurtion
talosctl gen config fyc --dns-domain k8s.lan --install-disk /dev/sda https://$MASTER_IP:6443 --additional-sans $MASTER_IP,master01.k8s.lan 

# Appliquer la configuration généré sur le master pour la première fois
talosctl apply-config --insecure --nodes $MASTER_IP --file controlplane.yaml

# Appliquer la configuration généré sur les workers pour la première fois
talosctl apply-config --insecure --nodes $WORKER_IP --file worker.yaml


```
