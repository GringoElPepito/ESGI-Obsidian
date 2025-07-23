Chiffrement de disques
soit via une passphrase
soit via puce TPM


`/boot` : contient les fichiers de démarrage

Options de montage :
- nodev : empêche de pouvoir monter un périphérique sur la partition
- nosuid : empêche l'ajout du suid sur les fichires présent dans le point de montage
- noexec : empêche l'exécution de binaires sur la partition


Il faut séparer dans des partitions primaires les dossier suivants :
- `/boot`
- `/tmp`
- `/var/log`

Chiffrement AES minimum 256 bits

Possibilité de passé root via vim
```bash
sudo vim fichier
:shell
```

Pas mettre de mot de passe a root pour créer un utilisateur avec sudo

`/etc/fstab` :
```
uuid du disk  point de montage  option de montage  utilitaire dump  activer fsck
```
- uuid :
- point de montage
- 
- fsck : FileSystem Check si on met 1, le filesystem sera vérifier en premier généralement utilisé pour le /. Le 2 effectue aussi la réparation mais s'exécute seulement après avoir fini tous les disk en 1. 0 désactive la vérification du disque

La commande suivante permet de vérifier la conf du fichier `/etc/fstab` et de remonter les disques suites aux modifications apportés à ce dernier
```bash
sudo mount -a
```

Il ne faut pas modifier le fichier de configuration par défaut, il faut plutôt ajouter des fichiers dans le dossier à la configuration.
La configuration du service SSH doivent être créer dans le dossier `/etc/sshd/sshd_config.d`
Ceci est valable pour tous les packages.

```sshd_config
Port 100
LoginGraceTime 1m
PermitRootLogin no
StrictModes yes
MaxAuthTries 3
MaxSessions 5
AuhtorizedKeysFile .ssh/authorized_keys
PasswordAuthentication yes
PermitEmptyPasswords no
UsePAM yes
AllowAgentForwarding no
AllowTcpForwarding no
X11Forwarding no
PermitTTY yes
PrintMotd no
PrintLastLog yes
TCPKeepAlive no
PermitUserEnvironment no
AllowGroups sshusers
Protocol 2
```

- `Port` : permet de spécifier le port (si exposé sur internet mettre autre chose que 22)
- `LoginGraceTime` : le temps d'inactivité ()
- `PermitRootLogin` : Autorise ou non la connexion de root en ssh
- `StrictModes` : 
- `MaxAuthTries` : Nombre de tentatives de connexion maximum
- `MaxSessions` : Limite du nombre de session SSH simultanées
- `AuthorizedKeysFile` : 
- `X11Forwarding` : permet de déporter la configuration d'affichage sur le client SSH (à mettre sur `no`)
- `PermitTTY` : 
- `PrintMotd` : Affiche le message du jour
- `PrintLastLog` : Affiche la date de la dernière connexion
- `TCPKeepAlive` : 
- `AllowGroups` : Permet de spécifier un groupe dont les membres seront autorisé à se connecter (à renseigner)
- `Protocol` : permet de spécifier la version de SSH utiliser (mettre à 2)

Génération d'une paire de clé privé/public
```bash
ssh-keygen -t rsa -b 4096
```

Copie la clé publique sur une machine distante
```bash
ssh-copy-id -p 100 -f -i .ssh/id_rsa.pub debian@192.168.1.100
```

Les droits à mettre sur le dossier `.ssh` sont 0700 et 0600 sur les fichiers de clé publics

```bash
sudo firewall-cmd --list-all
```

```bash
sudo firewall-cmd --add-port=60000/tcp
```

```bash
sudo semanage port -a -t ssh_port_t -p tcp 60000
```