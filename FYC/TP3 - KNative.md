Reprendre le cluster Kubernetes installé au cours du TP2.



But du TP déployer un service FaaS sécurisé utilisant des microVM avec Knative et Firecracker sur un cluster Kubernetes Talos.

ajouter un disque de 20G sur chaque worker TALOS 

# Vérification

Vérifier la virtualisation du chaque worker
```bash
talosctl -n <WORKER_IP> dmesg | grep -iE 'kvm|vmx|svm'
talosctl -n <WORKER_IP> ls /dev/kvm
```

Labelisation des worker pour les réserver aux futurs workloads FaaS :
```bash
kubectl label node worker01 \
  faas-firecracker=true

kubectl label node worker02 \
  faas-firecracker=true

kubectl label node worker03 \
  faas-firecracker=true
  
kubectl taint node worker01 \
  workload=faas:NoSchedule

kubectl taint node worker02 \
  workload=faas:NoSchedule

kubectl taint node worker03 \
  workload=faas:NoSchedule
```