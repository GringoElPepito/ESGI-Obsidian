# BDD
```bash
sudo dnf in -y mariadb-server-galera
sudo firewall-cmd --add-service=mysql 
sudo firewall-cmd --add-port={3306/tcp,4567/tcp,4568/tcp,4444/tcp}
sudo firewall-cmd --runtime-to-permanent
sudo vi /etc/my.cnf.d/galera.cnf
sudo sed -i '/^wsrep_cluster_name="my_wsrep_cluster"/cwsrep_cluster_name="galera_cluster"' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^#wsrep_cluster_address="dummy:\/\/"
/cwsrep_cluster_address="gcomm:\/\/"' /etc/my.cnf.d/galera.cnf
```