Le réseau est l'ensemble des moyens matériels et logiciels permettant à deux machines informatiques de communiquer. Au fils des années les réseaux informatiques pour permettre plus de communications et de plus haut débits.

Le réseau se pense lui aussi en fonction des besoins auxquelles doit répondre l'infrastructure. On ne conçoit pas une architecture réseau de la même manière pour un site de production industriel ou pour héberger une application web.

Architecture
Accès Internet
Haute disponibilité Réseau
# Architecture LAN

Les architectures réseaux ont grandement évolués au cours des années notamment pour s'adapter aux besoins des infrastructures. Au début des réseaux informatiques il existait 3 architectures : Le bus, l'anneau et l'étoile.

Le bus connecte toutes les machines à l'aide d'un seul et unique câble. De ce fait, chaque données transmises sur le réseau est en réalité transmis à l'ensemble des équipements reliés à ce bus.

L'anneau connecte chaque machine à deux autres machines uniquement de manière à former physiquement un anneau avec toutes les connexions. 

L'étoile connecte toutes les machines à un équipement central assurant la communication entre toutes les machines.
Liste des architectures :
- Bus -> Un seul câble sert à relier l'ensemble des machines du réseau. De ce fait, chaque données transmises sur le réseau est en réalité transmis à l'ensemble des équipements reliés à ce bus.
	- Avantages
		- La mise en place est très simple
		- Peu de matériel est nécessaire pour son installation donc très accessible
	- Inconvénients
		- SPOF (Single Point Of Failure), si le câble est endommagé, l'intégralité du réseau est hors-service.
		- Cette architecture peut présenter un très grand risque de collision en fonction du nombre de machine connecté pouvant amener à des pertes de paquets ou une congestion du réseau.
		- Chaque données étant transmises à toutes les machines connectées, n'importe qui peut récupérer le trafic y circulant.
- Anneau -> Chaque appareil est relié à deux autres terminaux uniquement de telle sorte à former physiquement une boucle une fois toutes les machines connectés.
	- Avantages
		- A
	- Inconvénients
- Etoile -> Tous les terminaux sont reliés à un équipement unique assurant la communication entre toutes les machines.
	- Avantages
		- a
	- Inconvénients
- 3 tiers -> Modèle se découpant en 3 couches hiérarchiques, la couche access, la couche distribution/agrégation et la couche coeur. La couche access fournit l'accès réseau aux terminaux (poste utilisateur, imprimantes, téléphone IP etc...) généralement grâce à des switchs de niveaux 2. La couche distribution fait le lien entre la couche coeur et la couche access. C'est à ce niveau qu'on assure le routage inter-VLANs et les filtrages (ACL) notamment grâce aux switches de niveaux 3. Enfin la couche coeur sert à faire transiter le trafic entre les différents équipements de la couche distribution avec des très hauts débits.
	- Avantages
		- Chaque équipement à un rôle définit ce qui permet de faciliter l'identification d'une panne réseau.
		- Cette architecture possède aussi une assez grande capacité d'évolution.
		- De plus c'est une infrastructure particulièrement adapté au trafic nord-sud, 
	- Inconvénients
- 2 tiers (collasped) On repart du modèle 3 tier, cependant on fait ici fusionner la couche coeur et la couche distribution. De cette manière on simplifie l'architecture et réduit son coût.
	- Avantages
		- a
	- Inconvénients
- Spine and leaf (Clos) -> les leafs servent à connecter les terminaux (Serveurs, stockages, firewalls etc...) au reste du réseau. Chaque leaf est relié à tous les spines. les spines servent uniquement à faire transiter le trafic entre les leafs. D'ailleurs les spines sont uniquement reliés aux leafs.
	- Avantages
		- a
	- Inconvénients
- Fat tree -> augmentation du nombre de lien à chaque fois que l'on remonte une couche.
	- Avantages
		- a
	- Inconvénients
- 5 clos/Super spine -> est une extension de l'artchitecture spine and leaf permettant de dépasser la limité d'évolution imposé par le nombre de ports disponibles ou la complexité du cablâge. On va découper l'architecture en Pod, chaque pod sera un réseau spine-leaf. Chacun des spines de ces pods remontent sur les Super-Spines qui ont pour but d'interconnecter tous les pods.
	- Avantages
		- a
	- Inconvénients

# Architecture WAN