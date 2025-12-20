A réaliser sur chaque node :

```bash
apt install open-iscsi # installation du package
iscsiadm -m discovery -t sendtargets -p 10.99.99.2 # recherche des targets
iscsiadm -m node -T iqn.2025.local.truenas:proxmox01 -p 10.99.99.2 --login # connexion à la target
lsblk # vérification de la présence d'un nouveau disque
pvcreate /dev/sda # initialisation du disque
vgcreate vg_iscsi_vm_datastore /dev/sdb # création d'un volume group
lvcreate -l 100%FREE -T vg_iscsi_vm_datastore/data # création d'un thin pool
pvesm add lvmthin truenas-iscsi \ # ajout du LVM-Thin iSCSI en tant que datastore
  --vgname vg_iscsi_truenas \
  --thinpool data
```

