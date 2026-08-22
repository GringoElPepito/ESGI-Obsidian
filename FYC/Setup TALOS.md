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

Récupération des disques :
```bash
talosctl get disks --insecure --nodes %MASTER_IP%
```

Il faut ensuite récupéré le disque principal dans la majorité des cas ce sera `/dev/sda` 

Génération des fichier de configuration :
```bash
talosctl gen config fyc --dns-domain k8s.lan --install-disk /dev/sda https://$MASTER_IP:6443
```

Appliquer la configuration généré sur le master pour le premier démarrage :
```bash
talosctl apply-config --insecure --nodes $MASTER_IP --file controlplane.yml
```


