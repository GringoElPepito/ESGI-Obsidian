# RSYSLOG

## SRV-RSYSLOG
```bash
IP_CLIENT="debian@192.168.206.140"
sudo apt update -y && sudo apt upgrade -y
sudo apt install rsyslog rsyslog-gnutls rsyslog-relp openssl ca-certificates easy-rsa vim -y
mkdir ~/easy-rsa
ln -s /usr/share/easy-rsa/* ~/easy-rsa/
chmod 700 ~/easy-rsa
cd ~/easy-rsa
./easyrsa init-pki
echo 'set_var EASYRSA_REQ_COUNTRY "FR"
set_var EASYRSA_REQ_PROVINCE "Ile-de-France"
set_var EASYRSA_REQ_CITY "Paris"
set_var EASYRSA_REQ_ORG "ESGI"
set_var EASYRSA_REQ_EMAIL "admin@ludovik.ovh"
set_var EASYRSA_REQ_OU "Community"
set_var EASYRSA_ALGO "rsa"
set_var EASYRSA_DIGEST "sha512"
' > pki/vars
./easyrsa build-ca
sudo scp pki/ca.crt $IP_CLIENT:/tmp
sudo cp pki/ca.crt /usr/local/share/ca-certificates/
sudo update-ca-certificates
mkdir ~/practice-csr
cd ~/practice-csr
openssl genrsa -out client-rsyslog.key
openssl req -new -key client-rsyslog.key -out client-rsyslog.req -subj "/C=FR/ST=Île-de-France/L=Paris/O=ESGI/OU=SRC/CN=client-rsyslog/emailAddress=admin@ludovik.ovh"
openssl req -in client-rsyslog.req -noout -subject
cp client-rsyslog.req /tmp
cd ~/easy-rsa
./easyrsa import-req /tmp/client-rsyslog.req client-rsyslog
./easyrsa sign-req server client-rsyslog
sudo scp pki/issued/client-rsyslog.crt $IP_CLIENT:/tmp
cd ~/practice-csr/
sudo scp ./client-rsyslog.key $IP_CLIENT:/tmp
openssl genrsa -out srv-rsyslog.key
openssl req -new -key srv-rsyslog.key -out srv-rsyslog.req -subj "/C=FR/ST=Île-de-France/L=Paris/O=ESGI/OU=SRC/CN=srv-rsyslog/emailAddress=admin@ludovik.ovh"
sudo cp srv-rsyslog.req /tmp
cd ~/easy-rsa
./easyrsa import-req /tmp/srv-rsyslog.req srv-rsyslog
./easyrsa sign-req server srv-rsyslog
sudo cp pki/ca.crt /etc/rsyslog.d
sudo cp pki/issued/srv-rsyslog.crt /etc/rsyslog.d
cd ~/practice-csr/
sudo cp srv-rsyslog.key /etc/rsyslog.d
sudo sed -i '/^#module(load="imtcp")/c\
module(load="imrelp") \
input( \
    type="imrelp" \
    port="20514" \
    tls="on" \
    tls.caCert="/etc/rsyslog.d/ca.crt" \
    tls.myCert="/etc/rsyslog.d/srv-rsyslog.crt" \
    tls.myPrivKey="/etc/rsyslog.d/srv-rsyslog.key" \
    tls.authMode="name" \
    tls.permittedpeer=["*"] \
)' /etc/rsyslog.conf
sudo sed -i '/^\$IncludeConfig \/etc\/rsyslog\.d\/\*\.conf/a \$template RemoteLogs,"/var/log/remote/%HOSTNAME%.log"' /etc/rsyslog.conf
echo '*.* ?RemoteLogs' | sudo tee -a /etc/rsyslog.conf
sudo mkdir /var/log/remote
sudo systemctl enable --now rsyslog
```


## CLIENT-RSYSLOG


```bash
IP_SRV=192.168.206.141
sudo apt update -y && sudo apt upgrade -y
sudo apt install rsyslog rsyslog-gnutls rsyslog-relp ca-certificates vim -y
sudo mkdir /usr/local/share/ca-certificates
sudo cp /tmp/ca.crt /usr/local/share/ca-certificates/
sudo update-ca-certificates
sudo mv /tmp/ca.crt /etc/rsyslog.d/ca.crt
sudo mv /tmp/client-rsyslog.crt /etc/rsyslog.d/client-rsyslog.crt
sudo mv /tmp/client-rsyslog.key /etc/rsyslog.d/client-rsyslog.key
sudo sed -i '/^#input(type="imtcp" port="514")/c\
\
module(load="omrelp") \
action( \
        type="omrelp" \
        target="'"$IP_SRV"'" \
        port="20514" \ 
        tls="on" \
        tls.caCert="/etc/rsyslog.d/ca.crt" \
        tls.myCert="/etc/rsyslog.d/client-rsyslog.crt" \
        tls.myPrivKey="/etc/rsyslog.d/client-rsyslog.key" \
        tls.authmode="name" \
        tls.permittedpeer=["*"] \
)' /etc/rsyslog.conf
sudo systemctl enable --now rsyslog
```
  

# Tâches planifiées

## Tâche 1

`/lib/systemd/system/task1.timer`:

```bash
[Unit]
Description=Task 1 timer

[Timer]
AccuracySec=1us
RandomizedDelaySec=1800
FixedRandomDelay=false
Persistent=true
RemainAfterElapse=true
OnBootSec=1us

[Install]
WantedBy=timers.target
```

`/lib/systemd/system/task1.service`

```bash
[Unit]
Description=Task 1 service

[Service]
ExecStart=/bin/bash -c 'logger tâche 1 ok'

[Install]
WantedBy=multi-user.target
```
## Tâche 2

### Script

`/opt/task2.sh`

```bash
#!/bin/bash
set -e #Indique que l'exécution doit s'arrêter si un autre code retour que 0 est reçu
[ ! -d "/var/log/task2" ] && mkdir /var/log/task2
apt-get update > /var/logtask2/task-$(date +%d-%m-%Y)
apt list --upgradable >> /var/log/task2/task-$(date +%d-%m-%Y)
```

### Cron

`/etc/cron.d/task1` :

```
00 12 * * 7 root /opt/task2.sh
```

### Systemd

`/lib/systemd/system/task2.timer`

```bash
[Unit]
Description=Task 2 timer

[Timer]
AccuracySec=1us
onCalendar= Sun *-*-* 12:00:00
Persistent=true
RemainAfterElapse=true  

[Install]
WantedBy=timers.target
```

`/lib/systemd/system/task2.service`

```bash
[Unit]
Description=Task 2 service

[Service]
ExecStart=/opt/task2.sh

[Install]
WantedBy=multi-user.target
```

## Tâche 3

### At

```bash
sudo at now + 5 minutes
at> logger 'lancement de la tâche ponctuelle'
at> echo test > /opt/mytask
```
## Tâche 4

`/lib/systemd/system/task4.timer`

```bash
[Unit]
Description=Task 4 timer

[Timer]
AccuracySec=1us
OnUnitActiveSec=1s
OnBootSec=1s
Persistent=true
RemainAfterElapse=true
Restart=always

[Install]
WantedBy=timers.target
```

`/lib/systemd/system/task4.service`

```bash
[Unit]
Description=Task 4 service

[Service]
ExecStart=/bin/bash -c 'echo "computer started"'

[Install]
WantedBy=multi-user.target
```

Commande

```bash
sudo systemctl enable --now task4.timer
```  
  
## Tâche 5

`/lib/systemd/system/task5.timer`

```bash
[Unit]
Description=Task 5 timer

[Timer]
AccuracySec=1s
OnCalendar=daily
Persistent=true
RemainAfterElapse=true

[Install]
WantedBy=timers.target
```

`/lib/systemd/system/task5.service`

```bash
[Unit]
Description=Task 5 service

[Service]
ExecStart=/bin/bash -c 'logger $(date +%d/%m/%Y)'

[Install]
WantedBy=multi-user.target
```

# Script

base commande :

```bash
sudo vim /usr/local/sbin/user-and-ui
sudo chmod +x /usr/local/sbin/user-and-ui
```

```bash

#!/bin/bash

set -e
loop=1
while [[ $loop -ne 0 ]]; do
    loop=0
    if [[ "$EUID" -ne 0 ]]; then
        echo "Le script doit être lancé en tant que root."
        exit 1
    fi
    for i in {1..10}; do
        username="utilisateur$i"
        if id -u "$username" > /dev/null 2>&1; then
            echo "Utilisateur $username déjà créé."
        else
            useradd -ms /bin/bash $username
            if id -u "$username" > /dev/null 2>&1; then
                echo "Ok pour $username."
            else
                echo "Une erreur s'est produite."
                exit 1
            fi
        fi
        echo 'utilisateur suivant'
    done
    choice='0'
    package=''
    gui=''
    while [[ $choice -ne '4' ]]; do
        echo 'Veuillez choisir votre interface graphique :'
        echo '- 1 pour KDE'
        echo '- 2 pour XFCE'
        echo '- 3 pour MATE'
        echo '- 4 pour quitter'
        read -p 'Choix : ' choice
        if [[ $choice -eq 1 ]]; then
            package="kde-plasma-desktop plasma-nm lightdm"
            gui="KDE"
        elif [[ $choice -eq 2 ]]; then
            package="xfce4 xfce4-goodies lightdm"
            gui="XFCE"
        elif [[ $choice -eq 3 ]]; then
            package="mate-desktop-environment lightdm"
            gui="MATE"
        elif [[ $choice -eq 4 ]]; then
            echo 'Au revoir'
            exit 0
        else
            echo "Votre choix n'est pas valide"
            echo ''
            continue
        fi
        if dpkg -s $package > /dev/null 2> /dev/null; then
            echo "$gui est déjà installé"
        else
            apt install -y $package 3> /dev/null 2> /dev/null > /dev/null || {
                echo "Une erreur s'est produite lors de l'installation des paquets"
                loop=1
                continue
            }
            echo "L'interface graphique $gui a bien été installée"
        fi
        choice=4
    done
done
exit 0
```