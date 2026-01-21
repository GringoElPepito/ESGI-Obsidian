Se configure dans Datacenter -> SDN

Tout d'abord il y a les zones, chaque zone est un conteneur qui va permettre d'enregistrer différents réseaux et sous-réseaux. Il 5 types de zone :
- simple -> Zone dont la portée se limite au nœud lui même. Non liable à une interface réseau (Physique ou virtuelle) des nœuds où sera diffusé la zone
- VLAN -> Zone partagée entre tous les nœuds sur lesquels elle existe. Liable à une interface réseau (Physique ou virtuelle) des nœuds où sera diffusé la zone
- QinQ -> Zone partagée entre tous les nœuds sur lesquels elle existe. Liable à une interface réseau (Physique ou virtuelle) des nœuds où sera diffusé la zone
- VxLAN -> Zone partagée entre tous les nœuds sur lesquels elle existe. Liable à une interface réseau (Physique ou virtuelle) des nœuds où sera diffusé la zone
- EVPN -> Zone partagée entre tous les nœuds sur lesquels elle existe. Liable à une interface réseau (Physique ou virtuelle) des nœuds où sera diffusé la zone

La création d'une zone ajoutera un élément Zone aux objets présents au sein des nodes cibles.

Après avoir créer votre zone, il va être possible de créer un VNet. Un VNet est un switch virtuel lié à une Zone et permettant de créer des sous-réseaux.

Les sous-réseaux offrent les fonctionnalités suivantes :
- Mis en place d'un DHCP Uniquement pour les zones simples
- SNAT permettant aux VM/LXC de sortir du sous-réseau 



Zone VLAN -> VNets si activation de l'option `VLAN AWARE` alors possible de définir le tag du VLAN sur les guests à ce moment le trafic sortant des guests sera taggué. Si `VLAN AWARE` est désactivé sur le VNet alors impossible de tagguer l'interface des guests (ERREUR au démarrage).


SI SDN fonctionne pas