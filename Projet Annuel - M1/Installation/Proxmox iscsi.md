# Ajout partage iscsi
Installation du package si non présent :
```bash
apt install open-iscsi # installation du package
```

Récupérer les partages disponibles iscsi :
```bash
iscsiadm -m discovery -t sendtargets -p 10.99.99.2:3260
```

Ajout du partage iSCSI pour montage automatique au démarrage du service `open-iscsi.service` :
```bash
iscsiadm -m node -T iqn.2005-10.org.freenas.ctl:vm-datastore -p 10.99.99.2:3260 --op update -n node.startup -v automatic
```

Redémarrage du service :
```bash
systemctl restart open-iscsi.service 
```

Vérification de la présence d'un nouveau disque :
```bash
lsblk
```

Déconnecter l'hôte du partage iscsi :
```bash
iscsiadm -m node -p 10.99.99.2:3260 --logout
```
# Installation LVM on iscsi
Initialisation du disque :
```bash
pvcreate /dev/sda
```

Création d'un volume group :
```bash
vgcreate vg_iscsi_vm_datastore /dev/sdb 
```

Création d'un thin pool :
```bash
lvcreate -l 100%FREE -T vg_iscsi_vm_datastore/data 
```

Ajout du LVM-Thin iSCSI en tant que datastore :
```bash
pvesm add lvmthin truenas-iscsi \ 
  --vgname vg_iscsi_truenas \
  --thinpool data
```

# Installation ZFS on iscsi
## Version 1 -> pas très fonctionnel
Création du pool zfs :
```bash
zpool create -f -o ashift=12 zfs-pool
```

Puis ajout du stockage via interface web.

## Version 2 -> plus carré
Nécessite SSH activé sur TrueNAS
Un utilisateur par node Proxmox avec accès SSH
Ajout de la clé public sur les hôtes Proxmox :
```bash
ssh truenas_admin@10.99.99.2
```

Création d