# TEMP
```bash


```
# SRV-RSYSLOG
```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install rsyslog rsyslog-gnutls vim -y
sudo systemctl enable --now rsyslog
sudo sed -i 's/^#module(load="imtcp")/module(load="imtcp" StreamDriver.Name="gtls" StreamDriver.Mode="1" StreamDriver.AuthMode="anon")/' /etc/rsyslog.conf
sudo sed -i '/^#input(type="imtcp" port="514")/c\
input(type="imtcp" port="514")\
global(\
    defaultNetstreamDriverCAFile="/etc/rsyslog.d/rsyslog.crt"\
    defaultNetstreamDriverCertFile="/etc/rsyslog.d/rsyslog.crt"\
    defaultNetstreamDriverKeyFile="/etc/rsyslog.d/rsyslog.key"\
)\
' /etc/rsyslog.conf
sudo sed -i '/^\$IncludeConfig \/etc\/rsyslog\.d\/\*\.conf/a \$template RemoteLogs,"/var/log/remote/%HOSTNAME%.log"' /etc/rsyslog.conf
echo '*.* ?RemoteLogs' | sudo tee -a /etc/rsyslog.conf
sudo openssl req -x509 -newkey rsa:2048 -keyout /etc/rsyslog.d/rsyslog.key -out /etc/rsyslog.d/rsyslog.crt -days 365 -nodes
sudo mkdir /var/log/remote
sudo systemctl restart rsyslog
```
# CLIENT-RSYSLOG
```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install rsyslog rsyslog-gnutls vim -y
sudo systemctl enable --now rsyslog
sudo sed -i '/^#input(type="imtcp" port="514")/c\
\
global(DefaultNetstreamDriverCAFile="/etc/rsyslog.d/rsyslog.crt") \
action(type="omfwd" protocol="tcp" target="192.168.206.139" port="514" StreamDriver="gtls" StreamDriverMode="1" StreamDriverAuthMode="anon")' /etc/rsyslog.conf
sudo scp kaisen@192.168.206.139:/etc/rsyslog.d/rsyslog.crt /etc/rsyslog.d/rsyslog.crt
sudo systemctl restart rsyslog
```

# Tâche Planifié
## Tâche 1
`/lib/systemd/system/task3.timer`:
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

# Script
```bash

```