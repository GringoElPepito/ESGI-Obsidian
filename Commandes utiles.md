Rechercher des fichiers via le contenu :
```bash
find / -type f -exec grep -H "ansible@pbs!ansible" {} \; 
```

```bash
find / -type d -name zabbix.conf.php
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
/usr/sbin/haproxy -Ws -f /etc/haproxy/haproxy.cfg -f /etc/haproxy/
```

Lister les services en échec
```bash
`systemctl list-units --failed`
```