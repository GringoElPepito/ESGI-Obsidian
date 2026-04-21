Rechercher des fichiers via le contenu :
```bash
find ./.config/hypr -type f -exec grep -H "background" {} \; 
```

```bash
find / -type d -name ISO
```

Scanner les disques :
```bash
echo '- - -' | sudo tee -a /sys/class/scsi_host/host*/scan
```

