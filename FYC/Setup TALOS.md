Création des variables (LINUX) :
```bash
export MASTER_IP=172.16.0.100

export WORKER_IP=("172.16.0.101" "172.16.0.102" "172.16.0.103")
```

Création des variables (Windows) :
```bash
set MASTER_IP=172.16.0.100 
&& 
set WORKER_IP=("172.16.0.101" "172.16.0.102" "172.16.0.103")
```

Génération des fichier de configuration :
```bash
talosctl gen config fyc https://172.16.0.100:6443
```

Appliquer la configuration généré sur

Récupération des disques :
```bash
talosctl get disks --insecure --nodes %MASTER_IP%
```
