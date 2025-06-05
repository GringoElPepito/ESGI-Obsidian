# TEMP
```bash


```
# SRV-RSYSLOG
```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install rsyslog vim -y
sudo systemctl enable --now rsyslog
sudo sed -i 's/^#module(load="imtcp")/module(load="imtcp" StreamDriver.Name="gtls" StreamDriver.Mode="1" StreamDriver.AuthMode="anon")/' /etc/rsyslog.conf
sudo sed -i 's/^#input(type="imtcp" port="514")/input(type="imtcp" port="514" StreamDriver="gtls")/' /etc/rsyslog.conf
echo "" | sudo tee -a /etc/rsyslog.conf
echo "$template(name=\"RemoteLogs\" type=\"string\" string=\"/var/log/remote/%HOSTNAME%.log\") *.* ?RemoteLogs" | sudo tee -a /etc/rsyslog.conf
sudo mkdir /etc/rsyslog
sudo openssl req -x509 -newkey rsa:2048 -keyout /etc/rsyslog/rsyslog.key -out /etc/rsyslog/rsyslog.crt -days 365 -nodes
```
# CLIENT-RSYSLOG
```bash
sudo apt update -y && sudo apt upgrade -y

```

# Tâche Planifié
```bash

```

# Script
```bash

```