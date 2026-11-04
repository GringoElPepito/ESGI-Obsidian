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
- Serverless -> Le but est de comprendre le fonctionnement des solutions FaaS (Function as a Service).
- Conteneurisation & Orchestration ->  
- Virtualisation ->
- Physique -> L'objectif de cette couche est de comprendre le fonctionnement des différents composants constituants un ordinateur, les relations entre elles et de déterminer la manière de choisir ces composants. 
- Stockage -> Ici, le but est de visualiser les différents enjeux liés au stockage
- Réseau -> Cette section se concentre sur l'approche à suivre pour concevoir une infrastructure réseau permettant de répondre aux besoins des services Cloud.

## Résultats

## Aller plus loin

## Références

## Annexes (schéma et illustrations)
