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