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

La commande suivante permet de vérifier la conf du fichier `/etc/fstab` et de remonter les disques suit
```bash
sudo mount -a
```
