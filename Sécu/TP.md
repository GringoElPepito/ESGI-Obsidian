# TP 1
## Partie 1
Installation de Rocky Linux, création d'une partition dédié pour :
- `/boot` : partition primaire non chiffré avec les options `nodev,nosuid,noexec`
- `/tmp` : partition primaire chiffré avec les options `nodev,nosuid,noexec`
- `/var/log` : partition primaire chiffré avec les options `nodev,nosuid,noexec`
- `/` : partition primaire chiffré sans options

Les options de chaque partition sont à renseigné dans le fichier `/etc/fstab`, il faut sur chaque ligne remplacer la mention `defaults` par `nodev,nosuid,noexec`

## Partie 2
1. Il est possible de mettre un fichier binaire dans le dossier `/boot`, `/tmp` ou `/var/log` par contre il n'est pas possible d'exécuter un binaire contenu dans l'un de ces dossiers
2. La commande `mknod -m 0660 test b 254 4` fonctionne mais il n'est pas possible de monter la partition à partir de là

# TP 2

# TP 3
Modification du fichier `/etc/ssh/sshd_config`
```sshd_config
Protocol 2
Port 2022
PermitRootLogin no


```
# TP 4
Configuration : 
- VMnet 1 : Host-Only 10.10.10.0/24
- VMnet 2 : Host-Only 10.20.20.0/24
- VMnet 8 : NAT 192.168.0.0/24

R-Debian 
- Networks :
	- VMnet1 : 10.10.10.2
	- VMnet8 : DHCP
- Configuration : 
	- Réseau
	- Activation du routage des paquets
```bash

```

C-Debian - Networks :
- VMnet1 : DHCP - GW 10.10.10.2
```bash

```

R-Rocky 
- Networks :
	- VMnet2 : 10.20.20.2 
	- VMnet8 : DHCP 
- Configuration : 
	- Réseau
	- Activation du routage des paquets
```bash

```

C-Rocky - Networks :
- VMnet2 : DHCP - GW 10.20.20.2
```bash

```