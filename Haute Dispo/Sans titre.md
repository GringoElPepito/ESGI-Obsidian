# WEB

# WEB 1
```bash
sudo dnf in -y nginx php-fpm php-mysqlnd
sudo firewall-cmd --add-service=http
sudo firewall-cmd --add-service=https
sudo firewall-cmd --add-port={80/tcp,443/tcp}
sudo firewall-cmd --runtime-to-permanent
sudo systemctl enable --now nginx
sudo systemctl enable --now php-fpm
```

# BDD
## BDD 1
```bash
sudo dnf install -y mariadb-server mariadb-server-galera galera setools-console policycoreutils-python-utils vim
sudo firewall-cmd --add-service=galera --permanent 
sudo vi /etc/my.cnf.d/galera.cnf
sudo sed -i '/^wsrep_cluster_name="my_wsrep_cluster"/cwsrep_cluster_name="galera_cluster"' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^#wsrep_cluster_address="dummy:\/\/"/cwsrep_cluster_address="gcomm://"' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^#wsrep_node_name=/cwsrep_node_name="bdd1"' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^#wsrep_node_address=/cwsrep_node_address="10.1.1.6"' /etc/my.cnf.d/galera.cnf
sudo galera_new_cluster
sudo systemctl enable mariadb
sudo sed -i '/^wsrep_cluster_address="gcomm:\/\/"/cwsrep_cluster_address="gcomm://10.1.1.6,10.1.1.7,10.1.1.8"' /etc/my.cnf.d/galera.cnf
sudo mysql_secure_installation
sudo mysql -u root
CREATE DATABASE WordPress CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE USER 'wpdb'@'10.1.1.4' IDENTIFIED BY 'wpdb';
GRANT ALL PRIVILEGES ON WordPress.* TO 'wpdb'@'10.1.1.4';
CREATE USER 'wpdb'@'10.1.1.5' IDENTIFIED BY 'wpdb';
GRANT ALL PRIVILEGES ON WordPress.* TO 'wpdb'@'10.1.1.5';
```

## BDD 2

```bash
sudo dnf install -y mariadb-server mariadb-server-galera galera setools-console policycoreutils-python-utils vim
sudo firewall-cmd --add-service=galera --permanent 
sudo vi /etc/my.cnf.d/galera.cnf
sudo sed -i '/^wsrep_cluster_name="my_wsrep_cluster"/cwsrep_cluster_name="galera_cluster"' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^#wsrep_cluster_address="dummy:\/\/"/cwsrep_cluster_address="gcomm://10.1.1.6,10.1.1.7,10.1.1.8"' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^#wsrep_node_name=/cwsrep_node_name="bdd2"' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^#wsrep_node_address=/cwsrep_node_address="10.1.1.7"' /etc/my.cnf.d/galera.cnf
sudo mysql_secure_installation
sudo systemctl enable --now mariadb
```

## BDD 3

```bash
sudo dnf install -y mariadb-server mariadb-server-galera galera setools-console policycoreutils-python-utils vim
sudo firewall-cmd --add-service=galera --permanent 
sudo vi /etc/my.cnf.d/galera.cnf
sudo sed -i '/^wsrep_on=0/cwsrep_on=1' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^wsrep_cluster_name="my_wsrep_cluster"/cwsrep_cluster_name="galera_cluster"' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^#wsrep_cluster_address="dummy:\/\/"/cwsrep_cluster_address="gcomm://10.1.1.6,10.1.1.7,10.1.1.8"' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^#wsrep_node_name=/cwsrep_node_name="bdd3"' /etc/my.cnf.d/galera.cnf
sudo sed -i '/^#wsrep_node_address=/cwsrep_node_address="10.1.1.8"' /etc/my.cnf.d/galera.cnf
sudo mysql_secure_installation
sudo systemctl enable --now mariadb
```

# Correction

## Galera
Mettre 1 IP statique et un nom d'hôte
![[Pasted image 20260519145031.png]]
```bash
sudo dnf install -y mariadb-server mariadb-server-galera galera setools-console policycoreutils-python-utils vim
sudo vim /etc/my.cnf.d/galera.cnf
sudo firewall-cmd --add-service=galera --permanent
sudo firewall-cmd --reload
sudo galera
```

Conf `/etc/my.cnf.d/galera.conf` :
```bash
[galera]
# Mandatory settings
wsrep_on                 = ON
wsrep_provider           = /usr/lib64/galera/libgalera_smm.so
wsrep_cluster_name       = "MariaDB Galera Cluster"
wsrep_cluster_address    = gcomm://10.1.1.6,10.1.1.7,10.1.1.8
binlog_format            = row
default_storage_engine   = InnoDB
innodb_autoinc_lock_mode = 2
innodb_buffer_pool_size  = 128M
wsrep_node_name          = DB1
wsrep_node_address       = 10.1.1.6
wsrep_sst_method         = rsync
bind-address             = 0.0.0.0
```

```
[galera]
# Mandatory settings
wsrep_on                 = ON
wsrep_provider           = /usr/lib64/galera/libgalera_smm.so
wsrep_cluster_name       = "MariaDB Galera Cluster"
wsrep_cluster_address    = gcomm://10.1.1.6,10.1.1.7,10.1.1.8
binlog_format            = row
default_storage_engine   = InnoDB
innodb_autoinc_lock_mode = 2
innodb_buffer_pool_size  = 128M
wsrep_node_name          = DB2
wsrep_node_address       = 10.1.1.7
wsrep_sst_method         = rsync
bind-address             = 0.0.0.0
```

```
[galera]
# Mandatory settings
wsrep_on                 = ON
wsrep_provider           = /usr/lib64/galera/libgalera_smm.so
wsrep_cluster_name       = "MariaDB Galera Cluster"
wsrep_cluster_address    = gcomm://10.1.1.6,10.1.1.7,10.1.1.8
binlog_format            = row
default_storage_engine   = InnoDB
innodb_autoinc_lock_mode = 2
innodb_buffer_pool_size  = 128M
wsrep_node_name          = DB3
wsrep_node_address       = 10.1.1.8
wsrep_sst_method         = rsync
bind-address             = 0.0.0.0

```
