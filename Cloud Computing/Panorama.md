On parle de cloud lorsque les données sont stockés ou traités en dehors de l'entreprise.
xAAS - As A Service
IaaS - Infrastructure As A Service
PaaS - Plateform As A Service
SaaS - Software As A Service
## Infrastructure
Partie prise en charge en locale souligné

| On-Site            | IaaS             | PaaS             | SaaS           |
| ------------------ | ---------------- | ---------------- | -------------- |
| ==Applications==   | ==Applications== | ==Applications== | Applications   |
| ==Data==           | ==Data==         | ==Data==         | Data           |
| ==Runtime==        | ==Runtime==      | Runtime          | Runtime        |
| ==Middleware==     | ==Middleware==   | Middleware       | Middleware     |
| ==O/S==            | ==O/S==          | O/S              | O/S            |
| ==Virtualization== | Virutalization   | Virtualization   | Virtualization |
| ==Servers==        | Servers          | Servers          | Servers        |
| ==Storage==        | Storage          | Storage          | Storage        |
| ==Networking==     | Networking       | Networking       | Networking     |
LRS = Locally Redundance Storage 3 copies dans un seul datacenter.
ZRS = Zone Redundance Storage 3 copies dans 3 datacenter différent.

Tag azure permet de sortir bilan financier

Toute VM sur Azure doit être relié à un sous-réseau.
Un réseau est défini par son adresse space
Notre réseau sera nommé VNET1
dans notre cas 10.1.0.0 /16. Il faut au sein de ce réseau créer un sous-réseaux qui s'appelle default par défaut et qui sera 10.1.0.0/24


Dans onglet sécurité rien cocher on veut garder les SOUS

à la création d'un réseau, Azure créer un ressource groupe `NetworkWatcherRG` et qui permet de travailler avec le service NetworkWatcher qui permet de créer une topologie du réseau crée

Accessible depuis le menu Monitoring->Topologie

Les VM ne sont pas des ressources, Une VM est associé à une carte réseau qui est associé à un subnet, La VM est aussi associé à un disque. Il faudra aussi une IP public permettant d'accéder à la machine virtuelle
On appellera notre VM1
La carte réseau est nommé automatiquement à partir du nom de la VM associé vm1xXx, par défaut en client DHCP.
Sur le sous-réseaux Microsoft bloque 3 addresses IP :
- 10.1.0.1 ip machine physique
- 10.1.0.2 ip NAT
- 10.1.0.3 ip DHCP
La première adresse dispnoble est donc la 10.1.0.4
L'IP public sera nommé VM1-ip
Disk VM1_OSDISK_(RANDOM_STRING)

ressource supplémentaire
Schedule nommé shutdown schedule computevm-VM1 19:00 romance standardt time

Image VM : Windows Server 2022 Datacenter - Gen1

Premium SSd NvME Gen3
Ultra disk NvME Gen4


