Pré-requis :
- ISO TALOS
- talosctl (outil cli)
- kubectl (outil cli)
- helm (outil cli)

# Etape 1
Installer 4 VM Talos :
- master01
	- CPU : 2
	- RAM : 2Gb
	- Disk : 20Gb
	- Network : Bridge
- worker01
	- CPU : 2
	- RAM : 2Gb
	- Disk : 20Gb
	- Network : Bridge
- worker02
	- CPU : 2
	- RAM : 2Gb
	- Disk : 20Gb
	- Network : Bridge
- worker03
	- CPU : 2
	- RAM : 2Gb
	- Disk : 20Gb
	- Network : Bridge

Configurer le réseau des instances Talos 

# Etape 2
Dans un shell taper les commandes suivantes :
```bash
# Variable contenant l'IP du node Master
export MASTER_IP=172.16.0.100 
# Variable contenant les IP des nodes Worker
export WORKER_IP=("172.16.0.101" "172.16.0.102" "172.16.0.103") 

# Récupération des disques
talosctl get disks --insecure --nodes $MASTER_IP 

# Génération des fichiers de configurtion
talosctl gen config fyc --dns-domain k8s.lan --install-disk /dev/sda https://$MASTER_IP:6443 --additional-sans $MASTER_IP,master01.k8s.lan 

# Appliquer la configuration généré sur le master pour la première fois
talosctl apply-config --insecure --nodes $MASTER_IP --file controlplane.yaml

# Appliquer la configuration généré sur les workers pour la première fois
talosctl apply-config --insecure --nodes $WORKER_IP --file worker.yaml

# Initialisation de ETCD et démarrage des composants du control-plane
talosctl bootstrap --nodes $MASTER_IP

# Récupération de la configuration kubectl
talosctl kubeconfig --nodes $MASTER_IP

# déploiement des namespaces openfaas & openfaas-fn
kubectl apply -f https://raw.githubusercontent.com/openfaas/faas-netes/master/namespaces.yml

# Ajout du dépôt HELM officiel d'OpenFaaS
helm repo add openfaas https://openfaas.github.io/faas-netes/
helm repo update

# Déployer OpenFaaS CE en mode Operator
helm upgrade openfaas --install openfaas/openfaas \
  --namespace openfaas \
  --set functionNamespace=openfaas-fn \
  --set generateBasicAuth=true \
  --set operator.create=true

# Vérification de l'état du déploiement
kubectl rollout status -n openfaas deploy/gateway
kubectl rollout status -n openfaas deploy/gateway-operator

# Récupération du mot de passe et connexion avec faas-cli
export OPENFAAS_URL=http://$(kubectl get svc -n openfaas gateway-external -o jsonpath='{.spec.clusterIP}'):8080

PASSWORD=$(kubectl get secret -n openfaas basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode)

echo -n "$PASSWORD" | faas-cli login --username admin --password-stdin

# Déploiement d'une fonction de test
faas-cli store deploy nodeinfo
faas-cli list
faas-cli invoke nodeinfo
```
