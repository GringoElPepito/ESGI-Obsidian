# TP 1
## Partie 1
Installation de Rocky Linux, création d'une partition dédié pour :
- `/boot` : partition primaire non chiffré avec les options `nodev,nosuid,noexec`
- `/tmp` : partition primaire chiffré avec les options `nodev,nosuid,noexec`
- `/var/log` : partition primaire chiffré avec les options `nodev,nosuid,noexec`
- `/` : partition primaire chiffré sans options

Les options de chaque partition sont à renseigné dans le fichier `/etc/fstab`, il faut sur chaque ligne remplacer la mention `defaults` par `nodev,nosuid,noexec`

## Partie 2
1. I