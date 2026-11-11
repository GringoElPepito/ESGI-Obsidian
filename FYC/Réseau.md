Le réseau est l'ensemble des moyens matériels et logiciels permettant à deux machines informatiques de communiquer.
# Architecture LAN

Liste des architectures :
- Bus
- Anneau
- Etoile
- 3 tiers
- 2 tiers (collasped)
- Spine and leaf (Clos) -> les leafs servent à connecter les terminaux (Serveurs, stockages, firewalls etc...) au reste du réseau. Chaque leaf est relié à tous les spines. les spines servent uniquement à faire transiter le trafic entre les leafs. D'ailleurs les spines sont uniquement reliés aux leafs.
- Fat tree -> augmentation du nombre de lien à chaque fois que l'on remonte une couche.
- 5 clos/Super spine -> est une extension de l'artchitecture spine and leaf permettant de dépasser la limité d'évolution imposé par le nombre de ports disponibles ou la complexité du cablâge. On va découper l'architecture en Pod, chaque pod sera un réseau spine-leaf. Chacun de ces pods remontent sur 

# Architecture WAN