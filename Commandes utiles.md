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

Permet de créer un fichier de log grâce à `nohup` tout en mettant l'exécution en arrière place `&`
```bash
nohup apt update -y &
```

vérifier conf HAProxy :
```bash
haproxy -c -V -f /etc/haproxy/haproxy.cfg
```
