
# Depuis un LINUX
Création des variables :
```bash
export MASTER_IP=172.16.0.100

export WORKER_IP=("172.16.0.101" "172.16.0.102" "172.16.0.103")
```

Récupération des disques :
```bash
talosctl get disks --insecure --nodes $MASTER_IP
```

Il faut ensuite récupéré le disque principal dans la majorité des cas ce sera `/dev/sda` 

Génération des fichier de configuration :
```bash
talosctl gen config fyc --dns-domain k8s.lan --install-disk /dev/sda https://$MASTER_IP:6443
```

Appliquer la configuration généré sur le master pour le premier démarrage :
```bash
talosctl apply-config --insecure --nodes $MASTER_IP --file controlplane.yaml
```

Appliquer la configuration à chaud :
```bash
talosctl apply-config --nodes $MASTER_IP --file controlplane.yaml
```

Appliquer la configuration généré sur les worker pour le premier démarrage :
```bash
talosctl apply-config --insecure --nodes $WORKER_IP --file worker.yaml
```
# Depuis un Windows

Création des variables :
```bash
set MASTER_IP=172.16.0.100 
&& 
set WORKER_IP=("172.16.0.101" "172.16.0.102" "172.16.0.103")
```

Récupération des disques :
```bash
talosctl get disks --insecure --nodes %MASTER_IP%
```

Il faut ensuite récupéré le disque principal dans la majorité des cas ce sera `/dev/sda` 

