# Comment monter son propre Cloud ? du Hardware au Serverless
*Mémoire de master de Rafael FERNANDO, Alexis FRANCESCO, Julien ZEYBEL, Miguel FERREIRA*

## Contexte et état de l'art

Dans ce qui se fait dans la sphère du cloud computing, le scope est très large, mais il existe maintenant des briques stables qui permettent de créer et de manager un cloud, on peut citer les technologies, qui sont encore utilisé aujourd'hui et répandu mais également ce qui se fait de mieux selon les grands acteurs publique:

- L'hyperviseur KVM en grande partie dû au fait que c'est une partie intégrante du noyau linux et donc les améliorations faites au noyau Linux sont directement reflétées sur KVM. Il y a également le cas d'AWS Nitro (qui utilise un hyperviseur léger basé sur KVM) ou même [Firecracker](https://fyc.esgi.fr/mod/glossary/showentry.php?eid=7684&displayformat=dictionary "Glossaire/Lexique : Firecracker") VM (micro-VM) pour du lambda AWS.
- Kubernetes s’impose comme standard pour orchestrer les conteneurs.
- Le runtime de conteneurisation imposé comme standard de facto [containerd](https://fyc.esgi.fr/mod/glossary/showentry.php?eid=7668&displayformat=dictionary "Glossaire/Lexique : containerd").
- Le serverless sur Kubernetes avec Knative (en complément d'OpenFaaS), qui permet de scaler jusqu'à zéro quand l'application n'est pas utilisée.
- Le sandboxing de conteneurs avec [gVisor](https://fyc.esgi.fr/mod/glossary/showentry.php?eid=7687&displayformat=dictionary "Glossaire/Lexique : gVisor") ou [Kata Containers](https://fyc.esgi.fr/mod/glossary/showentry.php?eid=7693&displayformat=dictionary "Glossaire/Lexique : Kata Containers") (conteneurs dans des micro-VMs), qui renforce l'isolation entre les clients sur une même machine.
- Le stockage distribué avec Ceph (bloc, objet, fichier) et l'API S3, devenue le standard de fait du stockage objet (MinIO permet de la recréer en local).
- Le réseau avec les CNI (Calico, Cilium basé sur [eBPF](https://fyc.esgi.fr/mod/glossary/showentry.php?eid=7681&displayformat=dictionary "Glossaire/Lexique : eBPF")) pour les clusters, et le SDN/overlay (Open vSwitch, OVN, VXLAN) pour isoler les réseaux de chaque client.
- L'Infrastructure as Code avec Terraform/OpenTofu et Ansible, ainsi que le [GitOps](https://fyc.esgi.fr/mod/glossary/showentry.php?eid=7686&displayformat=dictionary "Glossaire/Lexique : GitOps") (Argo CD, Flux) où Git devient la source de vérité de l'infrastructure.
- La gestion des machines physiques avec OpenStack (qui permet de gérer un parc comme un IaaS) et le provisionnement bare metal (MAAS, Ironic, PXE).
- L'observabilité avec Prometheus, Grafana et OpenTelemetry pour superviser des systèmes distribués.
- Le standard OCI pour les images et runtimes de conteneurs, qui garantit l'interopérabilité entre les outils.

## Sujet et problématique

Le cloud 

## Méthode

L'approche utilisé est de descendre chacune des couches composants une infrastructure cloud. les couches se découpent de la manière suivante :
- Serverless
- Conteneurisation et orchestration
- Virtualisation
- Physique
- Stockage
- Réseau

### Physique

Description des différents composants d'un ordinateur et de leur rôle :
- CPU : Composant polyvalent réalisant des calculs complexes. Traite peu d'opération à la fois mais le fait à une très grande vitesse.

- RAM : Composant servant à stocker de manière temporaire les informations nécessaires aux calculs du processeur. Ce type de mémoire propose une très faible latence ce qui parfait pour accès rapide aux données stockés dans la RAM par le processeur. Les données enregistrées dans la RAM ne sont pas conservés entre les redémarrages ou lorsque la machine est éteinte.
- GPU : Composant permettant de réaliser de nombreux calculs simples simultanément (calculs matricielles). Utiliser pour l'affichage ou l'IA.

- VRAM : Composant généralement directement intégré à la carte graphique et rempli le même rôle que la RAM mais pour la carte graphique. Ce type de mémoire possède une très large bande passante permettant de transmettre une large quantité de données au GPU parfait pour que celui-ci réalise ces nombreux calculs simultanément. A l'instar de la RAM, la VRAM ne conserve pas non plus les données entre les redémarrage ou lorsque la machine est éteinte.

- Stockage : Composant servant à conserver les données et ce même lorsque la machine n'est plus alimentée. Différents format (HDD, SSD SATA, SSD NVMe, Clé USB etc...)

- Carte mère : Composant central regroupant tous les autres composants de l'ordinateur. Il intègre le BIOS ou l'UEFI qui est le système à la toute base de n'importe quel ordinateur qui fournit un accès direct aux autres composants.

- Carte réseau : Composant permettant de connecter la machine à un réseau pour que celle-ci puisse communiquer avec d'autres machines. La majorité des cartes mères intègrent directement une ou plusieurs cartes réseaux.

- Alimentation : Composant servant à fournir l'énergie nécessaire au fonctionnement de l'ensemble des composants de l'ordinateur.

### Stockage

Les RAID (Redundant Array of Independant Disks) sont des manières d'associer différents supports physiques de stockage dans le but d'améliorer les performances et la résilience du stockage.

L'architecture de stockage définit le chemin que vont emprunter les serveurs de calculs pour accéder aux stockages. Il existe 5 architectures :
- DAS (Direct Attaced Storage)
- NAS (Network Attached Storage)
- SAN (Storage Area Network)
- HCI (Hyperconverged Infrastructure)
- Cloud

Le mode de stockage correspond à la manière dont les données sont visualisées par le serveur de calcul au sein d'un espace de stockage. Il existe 3 modes :
- Stockage par bloc
- Stockage par fichier
- Stockage par objet


## Résultats

## Aller plus loin

## Références

## Annexes (schéma et illustrations)
