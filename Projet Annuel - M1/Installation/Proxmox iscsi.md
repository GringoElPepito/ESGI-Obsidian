Installation du package si non présent :
```bash
apt install open-iscsi # installation du package
```
Ajout du partage iSCSI pour montage automatique au démarrage du service `open-iscsi.service` :
```bash
iscsiadm -m node -T iqn.2005-10.org.freenas.ctl:vm-datastore -p 10.99.99.2 --op update -n node.startup -v automatic
```
redémarrage du service :
```bash
systemctl restart open-iscsi.service 
```
vérification de la présence d'un nouveau disque :
```bash
lsblk
```
initialisation du disque :
```bash
pvcreate /dev/sda
```
création d'un volume group :
```bash
vgcreate vg_iscsi_vm_datastore /dev/sdb 
```
création d'un thin pool :
```bash
lvcreate -l 100%FREE -T vg_iscsi_vm_datastore/data 
```
ajout du LVM-Thin iSCSI en tant que datastore :
```bash
pvesm add lvmthin truenas-iscsi \ 
  --vgname vg_iscsi_truenas \
  --thinpool data
```