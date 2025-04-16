# Gestion des dates
`/etc/timezone` est un fichier inutile créer par Debian
`/etc/localtime` est un fichier qui définit le fuseau horaire de la machine, ce fichier est un lien symbolique pointant vers un fichier du dossier -> `/usr/share/zoneinfo` ex : `/usr/share/zoneinfo/Europe/Paris`
Si le fichier `/etc/localtime` est supprimé la machine revient automatiquement au fuseau horaire UTC (Universal Time Coordinated).

```
sudo ln -sfr /usr/share/zoneinfo/America/New_York /etc/localtime
```

```bash
sudo timedate set-timezone America/New_York
```