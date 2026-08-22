Création des variables :
```bash
export MASTER_IP=172.16.0.100

export WORKER_IP=("172.16.0.101" "172.16.0.102" "172.16.0.103")
```

Génération des fichier de configuration :
```bash
talosctl gen config fyc https://172.16.0.100:6443
```

Récupération des disques :
```bash
talosctl get disks --insecure --nodes 172.16.0.100
```