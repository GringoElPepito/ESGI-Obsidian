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
template(name="RemoteLogs" type=" string"string="/var/log/remote/%HOSTNAME%.log")
' /etc/rsyslog.conf
echo '*.* ?RemoteLogs' | sudo tee -a /etc/rsyslog.conf
sudo openssl req -x509 -newkey rsa:2048 -keyout /etc/rsyslog.d/rsyslog.key -out /etc/rsyslog.d/rsyslog.crt -days 365 -nodes
sudo systemctl restart rsyslog
```
# CLIENT-RSYSLOG
```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install rsyslog rsyslog-gnutls vim -y
sudo systemctl enable --now rsyslog
echo '
$DefaultNetstreamDriverCAFile /etc/rsyslog/rsyslog.crt
$ActionSendStreamDriver gtls
$ActionSendStreamDriverMode 1
$ActionSendStreamDriverAuthMode anon*.* @@serveur-central-ip:514
' | sudo tee -a /etc/rsyslog.conf
sudo scp kaisen@192.168.206.136:/etc/rsyslog.d/rsyslog.crt /etc/rsyslog.d/rsyslog.crt
sudo systemctl restart rsyslog
```

# Tâche Planifié
```bash

```

# Script
```bash

```