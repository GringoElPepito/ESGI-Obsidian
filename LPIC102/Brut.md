# Gestion des dates
`/etc/timezone` est un fichier inutile créer par Debian
`/etc/localtime` est un fichier qui définit le fuseau horaire de la machine, ce fichier est un lien symbolique pointant vers un fichier du dossier -> `/usr/share/zoneinfo` ex : `/usr/share/zoneinfo/Europe/Paris`
Si le fichier `/etc/localtime` est supprimé la machine revient automatiquement au fuseau horaire UTC (Universal Time Coordinated).

Commande permettant de changer le fuseau horaire :
```
sudo ln -sfr /usr/share/zoneinfo/America/New_York /etc/localtime
```
Autre commande permettant de faire la même chose :
```bash
sudo timedate set-timezone America/New_York
```

Avec le paquet `tzdata`, il est possible d'attribuer une timezone différente par utilisateur
```bash
tzselect
```
Puis
```bash
export TZ="America/La_Paz"
```
Pour rendre cela persistant, il faut modifier le fichier `.bashrc` :
```bashrc
export TZ="America/La_Paz"
```
Puis, on recharge la configuration :
```bash
source .bashrc
```

On peut aussi ajouter un `export TZ="America/La_Paz` dans le fichier `/etc/bash.bashrc` pour appliquer une timezone par défaut à tous les utilisateurs. 

Ordre de priorité :
1. Variable d'environnement Utilisateur
2. Variable d'environnement Système
3. Fichier `/etc/localtime`

# Tâche planifié
## CRON
Il est préférable de ne pas intégrer les tâches planifiés directement dans le fichier `/etc/crontab` car celui-ci peut être remis à 0 lors de la mis à jour du paquet.
On favorisera le fait de créer un fichier dans `/etc/cron.d`

## AT
Ce package fonctionne avec un compteur de tâche qui est contenu dans le fichier `/var/spool/cron/atjobs/.SEQ`

## SystemD
Il faut placer les fichiers `.timer` & `.service` dans le dossier `/lib/systemd/system/`
## Exo
### Tâche 1
`/etc/cron.d/task1` :
```
00 00 * * 7 root /opt/task1.sh
```

### Tâche 2
```bash
sudo apt install at -y
sudo at 12:00 2034-12-30
```

### Tâche 3
`/lib/systemd/system/task3.timer`:
```ini
[Unit]
Description=Task 3 timer

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

`/lib/systemd/system/task3.service`
```ini
[Unit]
Description=Task 3 service

[Service]
ExecStart=/bin/bash -c 'rm -rf /srv/.[^.]*'

[Install]
WantedBy=multi-user.target
```

### Tâche 4
`/etc/cron.monthly/task4`:
```bash
#!/bin/bash

set -e

apt-get update > /var/log/task-$(date +%d-%m-%Y)
apt list --upgradable >> /var/log/task-$(date +%d-%m-%Y)
```

# LOG 
Tous les logs sont stockés dans le dossier `/var/log`. Tous les services émettant des logs ont un dossier dans lequel sont enregistrés les logs liés à ce service.
`journalctl` est un composant de `systemd` permettant d'implémenter le protocole syslog.
`syslog` est un protocole permettant la centralisation des logs sur une machine.

Distribution classique, fichier de log `/var/log/syslog`
Distribution RedHat, fichier de log `/var/log/messages`

Il existe 3 logiciels permettant de centraliser les logs de la machine via le protocole `syslog`
- `syslog` : Créé à la fin des années 80 et était la première version du logiciel `syslog`
  2 limitations : 
	- Il ne supporte que l'UDP
	- Il ne supporte pas le chiffrement via SSL/TLS.
- `syslog-ng` : Créé à la fin des années 90 et est une amélioration de `syslog`. Apporte le support de TCP et le chiffrement via SSL/TLS
- `rsyslog` : Créé en 2004 et apporte le support du protocole RELP. 

`journalctl` génère des logs et les stockent `/var/log/journal/system.journal` qui est un fichier binaire non exécutable. Les données ne sont donc pas lisible en brut.

Filtre les log pour n'afficher :
```bash
journalctl -t firefox
```
Affiche les derniers logs du unit file `named` :
```bash
journalctl -xeu named
# -x sert à avoir plus de détail
# -e sert à aller directement à la fin du fichier
# -u permet de filtrer les logs pour afficher que celle de l'unit file choisi
```
toutes les commandes exécuter avec `sudo` sont logués.

Permet de lire tous les logs du fichier `/proc/kmsg` :
```bash
sudo dmesg
```

Affiche tous les périphériques connectés à la machine :
```bash
lspci
```

Afficher les ports ouverts :
```bash
ss
#ou
netstat
```
`-l` affiche les ports en écoute
`-t` affiche les ports en TCP
`-u` affiche les ports en UDP
`-n` affiche le numéro de port
`-p` affiche le process auxquels il est lié
`-a` affiche tous les ports