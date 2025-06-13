# TP 1
## Partie 1
Installation de Rocky Linux, création d'une partition dédié pour :
- `/boot` : partition primaire non chiffré avec les options `nodev,nosuid,noexec`
- `/tmp` : partition primaire chiffré avec les options `nodev,nosuid,noexec`
- `/var/log` : partition primaire chiffré avec les options `nodev,nosuid,noexec`
- `/` : partition primaire chiffré sans options

Les options de chaque partition sont à renseigné dans le fichier `/etc/fstab`, il faut sur chaque ligne remplacer la mention `defaults` par `nodev,nosuid,noexec`

## Partie 2
1. Il est possible de mettre un fichier binaire dans le dossier `/boot`, `/tmp` ou `/var/log` par contre il n'est pas possible d'exécuter un binaire contenu dans l'un de ces dossiers
2. La commande `mknod -m 0660 test b 254 4` fonctionne mais il n'est pas possible de monter la partition à partir de là

# TP 2

# TP 3

```sshd_config
```
# TP 4
