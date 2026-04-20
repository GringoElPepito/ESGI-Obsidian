Rechercher des fichiers via le contenu :
```bash
find / -type f -exec grep -H "freenas-proxmox" {} \; 
```

Scanner les disques :
```bash
echo '- - -' | sudo tee -a /sys/class/scsi_host/host*/scan
```

