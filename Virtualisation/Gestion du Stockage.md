# Les Types de stockages
- Stockage local qui correspondent aux disques internes à l'ESXi
- Stockage partagé correspondant au stockage proposé par une baie de stockage NAS ou SAN.
Les deux types peuvent accueillir des VMs mais on privilégiera le stockage partagé car il permet la haute disponibilité des VMs & la migration à chaud. 
Avec le stockage local si un ESXi tombe les VMs étant dessus ne pourront pas redémarrer sur un autre ESXi.
vSAN, solution propriétaire de VMWare, permet de faire un stockage partagé via des disques locaux.

Le système de fichier utilisé est VMFS, un système de fichier propriétaire de VMWare. Ce système de fichier fonctionne en cluster et permet à plusieurs ESXi de lire et écrire simultanément sur le périphérique.


Target = Stockage cible (NAS)
Initiateur = 
Software Adapter permet de simuler une carte réseau dédié au stockage, on lui indique le FQDN de la Target qui sera ici un LUN. Par défaut, il communique via le VMKernel Management