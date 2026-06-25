Définition du quorum proxmox à 1 :
```bash
pvecm expected 1
```

Création de partition de disque 
```bash
sgdisk -n 1:0:1T /dev/sda
```
- `-n` permet définir les paramètres de création de la partition
- `1` -> numéro de partition
- `0` -> commence la partition au premier block disponible
- `1T` -> Taille de la partition
- `/dev/sda` -> chemin vers le disque cible

Connection d'un disque/partition physique a une VM via un pass-through 
```bash
qm set 510002 -scsi2 /dev/sda2
```
- `qm` -> commande permettant d'intéragir avec qemu
- `set` -> argument permettant de définir un paramètre pour une VM
- `510002` -> id de la VM cible
- `-scsi2` -> type de stockage à ajouter + identifiant
- `/dev/sda2` -> chemin vers le disque/partition cible

Migration à chaud de VM Linux -> pas de perte de connexion
Migration à chaud de VM Windows -> perte de 1 paquet
Migration de LXC -> Perte de 1 à 3 paquets (pas possible de faire de la migration à chaud car Kernel dépendant)

Failover fonctionnel mais très lent -> 5m30 pour VM Windows 10 et 4m30 pour VM Linux

## Désinstallation Complète de CEPH

[Source](https://www.starwindsoftware.com/blog/remove-ceph-from-proxmox-ve/)

```bash
systemctl stop ceph-mon.target
systemctl stop ceph-mgr.target
systemctl stop ceph-mds.target
systemctl stop ceph-osd.target
systemctl disable ceph-mon.target
systemctl disable ceph-mgr.target
systemctl disable ceph-mds.target
systemctl disable ceph-osd.target
pveceph purge
rm -rf /etc/systemd/system/ceph*
killall -9 ceph-mon ceph-mgr ceph-mds ceph-osd
rm -rf /var/lib/ceph/mon/
rm -rf /var/lib/ceph/mgr/
rm -rf /var/lib/ceph/mds/
rm -rf /var/lib/ceph/osd/
ls -l /var/lib/ceph/
rm -rf /etc/ceph/*
rm -rf /etc/pve/ceph.conf
rm -rf /etc/pve/priv/ceph.*
apt purge ceph-mon ceph-osd ceph-mgr ceph-mds ceph-base ceph-mgr-modules-core
```
