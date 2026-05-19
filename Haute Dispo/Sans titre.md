# HAProxy

```bash
sudo dnf in -y haproxy vim keepalived
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-service=https --permanent
sudo firewall-cmd --add-service=mysql --permanent
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --reload
sudo vim /etc/haproxy/conf.d/lb.conf
sudo systemctl enable --now haproxy
sudo setsebool -P haproxy_connect_any on
```

# WEB

# WEB 1
```bash
sudo dnf in -y nginx php-fpm php-mysqlnd wget vim rsync unzip vim
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-service=https --permanent
sudo firewall-cmd --reload
sudo vim /etc/nginx/conf.d/web.conf
wget https://fr.wordpress.org/wordpress-latest-fr_FR.zip
sudo unzip wordpress-latest-fr_FR.zip -d /var/www
sudo systemctl enable --now nginx
sudo systemctl enable --now php-fpm
sudo systemctl restart nginx
sudo setsebool httpd_can_network_connect=1
sudo setsebool httpd_can_network_connect_db=1
```

`/etc/nginx/web.conf` :
```
server {
  listen 80 default_server;
  listen [::]:80 default_server;
  root /var/www/wordpress;

  index index.php index.html;
  server_name _;
  location / {
    try_files $uri $uri/ =404;
  }

  location ~ \.php$ {
    fastcgi_pass unix:/run/php-fpm/www.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
  }

  location ~ /\.ht {
    deny all;
  }
} 


```

# BDD
## BDD 1
```bash
sudo dnf install -y mariadb-server mariadb-server-galera galera setools-console policycoreutils-python-utils vim
sudo firewall-cmd --add-service=galera --permanent 
sudo firewall-cmd --reload
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
sudo firewall-cmd --reload
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
sudo firewall-cmd --reload
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
sudo galera_new_cluster
sudo systemctl enable --now mariadb
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
