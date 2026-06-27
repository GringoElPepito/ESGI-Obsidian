# Setup
Passer du repo entreprise au repo No-Subscription
```bash
apt update
apt upgrade -y
apt install sudo
useradd -ms /bin/bash -UG sudo
passwd ansible
proxmox-backup-manager user create ansible@pam
```