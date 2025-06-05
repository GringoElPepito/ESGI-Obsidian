# TEMP
```bash
#module(load="imtcp")
#input(type="imtcp" port="514")
```
# SRV-RSYSLOG
```bash
sudo apt update -y && sudo apt upgrade -y
sudo apt install rsyslog vim -y
sudo systemctl enable --now rsyslog
sudo sed -i 's/^#module(load="imtcp")/module(load="imtcp")/' /etc/rsyslog.conf
sudo sed -i 's/^#input(type="imtcp" port="514")/input(type="imtcp" port="514")/' /etc/rsyslog.conf
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