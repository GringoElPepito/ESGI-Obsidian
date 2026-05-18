# BDD
```bash
sudo dnf in -y mariadb-server-galera
sudo firewall-cmd --add-service=mysql 
sudo firewall-cmd --add-port={3306/tcp,4567/tcp,4568/tcp,4444/tcp}
sudo firewall-cmd --runtime-to-permanent
sudo vi /etc/my.cnf.d/galera.cnf
sudo sed -i ''
```