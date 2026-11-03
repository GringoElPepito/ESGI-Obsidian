Reprendre le cluster Kubernetes installé au cours du TP2.

But du TP déployer un service FaaS sécurisé utilisant des microVM avec Knative et Firecracker sur un cluster Kubernetes Talos.

ajouter un disque de 20G sur chaque worker TALOS 

# Vérification

Vérifier la virtualisation du chaque worker
```bash
talosctl -n <WORKER_IP> grep -E -o 'vmx|svm' /proc/cpuinfo | sort -u

talosctl -n <WORKER_IP> ls -l /dev/kvm
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

Créer le fichier `runtimeclass.yaml`
```YAML
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: kata
handler: kata-qemu-runtime-rs
overhead:
  podFixed:
    cpu: 250m
    memory: 256Mi
```

Puis appliquer-le :
```bash
kubectl apply -f runtimeclass.yaml
```

Vérifier que le runtime kata a bien été ajouté :
```bash
kubectl get runtimeclass
```

Créer le fichier `kata-test.yml` pour tester la création d'un première microVM :
```bash
apiVersion: v1
kind: Pod
metadata:
  name: kata-test
  namespace: default
spec:
  runtimeClassName: kata
  restartPolicy: Never
  containers:
    - name: test
      image: alpine:3.21
      command:
        - sh
        - -c
        - "uname -a; sleep 3600"
      resources:
        requests:
          cpu: 250m
          memory: 256Mi
        limits:
          cpu: "1"
          memory: 512Mi
      securityContext:
        allowPrivilegeEscalation: false
        capabilities:
          drop:
            - ALL
        runAsNonRoot: true
        runAsUser: 1000
        seccompProfile:
          type: RuntimeDefault
```

Puis appliquer-le :
```bash
kubectl apply -f kata-test.yaml
kubectl get pod kata-test -o wide
kubectl logs kata-test
```

Il faut ensuite vérifier que le noyau de la microVM est différent du noyau de l'hôte :
```bash
kubectl exec kata-test -- uname -r
```

Nativement OpenFaaS exécutera des pods classiques sans utiliser de MicroVM. Pour cela, nous allons installer Kyverno qui va permettre de modifier les ressources créées par OpenFaaS pour changer leur runtime par celui de kata et donc lancer nos fonctions serverless au sein de microVM.

Installation de Kyverno :
```bash
helm repo add kyverno \
  https://kyverno.github.io/kyverno/

helm repo update

helm upgrade --install kyverno kyverno/kyverno \
  --namespace kyverno \
  --create-namespace
```

Vérification de l'installation de kyverno :
```bash
kubectl get pods -n kyverno
```

Création d'une ClusterPolicy dans le fichier `openfaas-kata-policy.yaml` pour indiquer à Kyverno qu'il doit modifier les nouveaux pods créés par OpenFaaS :
```YAML
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: openfaas-use-kata
spec:
  background: false
  rules:
    - name: inject-kata-runtime
      match:
        any:
          - resources:
              kinds:
                - Pod
              namespaces:
                - openfaas-fn
      mutate:
        patchStrategicMerge:
          spec:
            runtimeClassName: kata
```

Puis appliquer-le :
```bash
kubectl apply -f openfaas-kata-policy.yaml
```

Créer une fonction python :
```bash
faas-cli new hello-kata --lang python3
```

Puis modifier le fichier `hello-kata/handler.py` :
```Python
import platform
import os

def handle(event, context):
    return {
        "message": "Hello from Kata microVM!",
        "kernel": platform.release(),
        "hostname": os.uname().nodename
    }
```

Construit l'image :
```bash
faas-cli build -f hello-kata.yml
```

Déploiement de la fonction :
```bash
faas-cli deploy -f hello-kata.yml
```

Test de la fonction :
```bash
echo '{"name":"LAB"}' | \
  faas-cli invoke hello-kata
```