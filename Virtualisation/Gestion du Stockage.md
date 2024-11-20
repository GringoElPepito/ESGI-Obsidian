# Les Types de stockages
- Stockage local qui correspondent aux disques internes à l'ESXi
- Stockage partagé correspondant au stockage proposé par une baie de stockage NAS ou SAN.
Les deux types peuvent accueillir des VMs mais on privilégiera le stockage partagé car il permet la haute disponibilité des VMs & la migration à chaud. 
Avec le stockage local si un ESXi tombe les VMs étant dessus ne pourront pas redémarrer sur un autre ESXi.
vSAN, solution propriétaire de VMWare, permet de faire un stockage partagé via des disques locaux.

Le système de fichier utilisé est VMFS, un système de fichier propriétaire de VMWare. Ce système de fichier fonctionne en cluster et permet à plusieurs ESXi de lire et écrire simultanément sur le périphérique.

vmhba = controleur de stockage (on peut aussi les considéré comme des cartes réseaux dédiés au stockage)

Target = Stockage cible (NAS/SAN)
Initiateur = les clients utilisant le stockage mis à disposition par la target (ESXi)
LUN = espace de stockage (généralement un ensemble de disque) proposé à des clients qui pourront installer leur propre système de fichier par dessus.
Software Adapter permet de simuler une carte réseau dédié au stockage, on lui indique le FQDN de la Target qui sera ici un LUN. Par défaut, il communique via le VMKernel Management

# Stockage VM
Il est possible d'augmenter la taille d'un disque d'une VM mais il ne peut y avoir de snapshot.


Pour augmenter la taille de la partition VMFS, on peut soit ajouter un autre disque soit étendre un disque déjà existant. Ces opérations peuvent être réalisé à chaud.

Prérequis vSAN :
- Licence
- Minimum 1 SSD par DiskGroup
- Un PortGroup VMKernel pour le vSAN
- Controler spécifique par ESXi reconnu par VMWare
vSAN offre plus granularité, permet de spécifié


