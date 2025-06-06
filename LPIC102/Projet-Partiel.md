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

Script:
```bash
#!/bin/bash
set -ex



if [[ "$EUID" -ne 0 ]]; then
	echo "Le script doit être lancé en tant que root."
	exit 1
fi

for i in {1..10}
do
	username="utilisateur$i"
	getent passwd $username > /dev/null
	if [ $? -eq 0 ]; then
		echo "Utilisateur $username déjà créé."
	else
		useradd -ms /bin/bash utilisateur$1
		getent passwd $username > /dev/null
		if [ $? -eq 0 ]; then
			echo "Ok pour $username."
		else
			echo "Une erreur s'est produite."
			exit 1
		fi
	fi
done

choice='0'
package=''
gui=''

install_package () {
	dpkg -s $package
	if [ $? -eq 1]; then
		echo "$gui est déjà installé"
		choix='4'
		return
	done
	apt install -y $package
}

while [ $choice -ne '4' ]
do
	echo 'Veuillez choisir votre interface graphique :'
	echo '- 1 pour KDE'
	echo '- 2 pour XFCE'
	echo '- 3 pour MATE'
	echo '- 4 pour quitter'
	read -p 'Choix : ' choice
	if [ $choice -eq '1']; then
		package="kde-plasma-desktop plasma-nm"
		gui="KDE"
	elif [ $choice -eq '2']; then
		package=""
		gui="XFCE"
	elif [ $choice -eq '3']; then
		package="mate-desktop-environment lightdm"
		gui="MATE"
	elif [ $choix -eq '4']; then
		echo 'Au revoir.'
		exit 0
	else
		echo 'Votre choix n\'est pas valide'
		echo ''
		continue
	fi

done

exit 0
```