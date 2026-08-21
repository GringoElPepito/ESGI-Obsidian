# Les composants informatiques

Les composants informatiques sont les éléments qui une fois assemblés permettent d'aboutir à ce qu'on appelle communément un ordinateur.
Voici la liste des principaux éléments composants un ordinateur :
- Le CPU ou processeur est un composant dit polyvalent qui excelle dans la réalisation les calculs dits complexes, il traite relativement peu d'opération à la fois mais le fait à une grande vitesse.
- La RAM ou mémoire vive qui va permettre de stocker de manière temporaire les informations nécessaires aux calculs du processeur. Ce type de mémoire propose une très faible latence ce qui parfait pour accès rapide aux données stockés dans la RAM par le processeur. Les données enregistrées dans la RAM ne sont pas conservés entre les redémarrages ou lorsque la machine est éteinte.
- Le GPU ou carte graphique ou processeur graphique est le composant qui se charge de résoudre de large quantité de calculs simples (généralement des calculs matricielles), ce qui est parfaitement adapter aux calculs graphiques (affichage) ou encore à l'intelligence artificielle.
- La VRAM ou mémoire vive graphique, est généralement directement intégré à la carte graphique et rempli le même rôle que la RAM mais pour la carte graphique. Ce type de mémoire possède une très large bande passante permettant de transmettre une large quantité de données au GPU. A l'instar de la RAM, la VRAM ne conserve pas non plus les données entre les redémarrage ou lorsque la machine est éteinte.
- Le Stockage ou mémoire morte se représente sous différents types de composants que nous détaillerons plus tard et qui permettent de conserver les données et ce même lorsque la machine n'est plus alimentée.
- La carte mère est le composant central d'un ordinateur, c'est grâce à elle que tous les autres composants peuvent communiquer ensemble. En plus de cela, la carte mère contient aussi le BIOS ou l'UEFI le système à la toute base de n'importe quel ordinateur qui fournit un accès direct aux autres composants.
- La carte réseau permet comme son nom l'indique de connecter la machine à un réseau pour que celle-ci puisse communiquer avec d'autres machines. La majorité des cartes mères intègrent directement une ou plusieurs cartes réseaux.
- L'alimentation est le composant permettant de fournir l'énergie nécessaire au fonctionnement de l'ensemble des composants de l'ordinateur.

Ils existent d'autres composant informatiques répondant à des besoins spécifiques que nous n'aborderons pas dans ce cours car ces derniers ne seront pas forcément pertinents par à rapport à notre cas.


# Comment choisir ces composants

Pour toute infrastructure informatique le choix du matériel est une question primordial. Celui-ci est dirigé par 2 éléments centraux le besoin auquel doit répondre l'infrastructure ainsi que le budget disponible. C'est d'ailleurs ce second facteur qui pose le plus souvent problème, ce qui est d'autant plus vrai avec toutes les récentes augmentations des prix des composants informatiques.

Il est important de choisir du matériel en cohérence avec les besoins définis pour l'infrastructure car un choix non adapté pourrait limiter les performances de cette dernière voir la rendre certaines fonctionnalités clés inexploitables.
C'est pourquoi il est impératif de comprendre le besoin à l'origine du projet informatique avant d'émettre n'importe quel choix.
Chaque service qu'accueillera l'infrastructure est susceptible d'avoir ses propres besoins, il est nécessaire de tous les prendre en compte et cela à chaque étape du design de l'infrastructure.

Avant de choisir le matériel il faut donc comprendre la charge principal auquel devra répondre l'infrastructure pour faire fonctionner le ou les services à héberger. Il faudra avant de choisir les composant, réaliser au préalable une estimation de la charge dans le but de définir un seuil de ressource permettant de répondre au besoin de l'infrastructure.

Prenons le cas de ce cours c'est à dire la mise en place d'un service d'exécution serverless. Ici la caractéristique la plus importante est la performance, le but va être de choisir un matériel permettant de réduire au maximum le temps entre le réception de la requête et l'envoi de la réponse par le serveur d'exécution. 
- CPU : Pour cela, nous allons donc privilégié des processeurs avec une haute fréquence quitte à prendre un modèle avec un peu moins de cœur (cela est a adapter en fonction de la charge estimée que devra encaisser l'infrastructure ainsi que du budget prévu pour celle-ci). Si jamais le processeur choisi est en-dessous des nécessités du service cela peut se traduire en un ralentissement global de ce dernier, car le processeur étant surchargé celui-ci mettra nécessairement plus de temps à traiter les requêtes.
- RAM : Il faut ensuite estimer la RAM nécessaire, comme nous visons la vitesse les plus hautes fréquences en DDR5 seront le mieux évidemment si le financement ne suit pas à ce moment-là la DDR4 pourra être une option de secours. Maintenant concernant la quantité de RAM en elle même, il faut réaliser une estimation du nombre. Sachant que la RAM doit augmenter en adéquation avec le nombre de cœur du processeur. La RAM est un élément centrale pour un FaaS car c'est la RAM qui est utilisé pour stocker les fonctions qui ont été récemment exécutés ainsi permettre de réduire le temps de réponse. Plus la RAM est rapide, plus la réponse le sera, plus il y a de RAM, plus il sera possible de stocker des fonctions qui seront rapidement exécutables.
-  Stockage : Concernant le stockage, vu que la vitesse est le critère recherché, nous nous orienterons donc vers des SSD NVMe si le budget le permet et dans le cas contraire des SSD SATA. Dans le cas du serverless, les données à stockées ne sont pas très lourdes, car il ne s'agit que de fichiers de codes que l'on associe à un identifiant. Cependant, ces données ont besoin d'être rapidement accessibles pour limiter l'impact du Cold Start.
- Réseau : l'objectif ici va être d'avoir la plus grosse bande passante possible, la première. On essaiera si possible pour la majorité des serveurs d'avoir au minimum 2 cartes réseau, la première servant au management du serveur, la seconde pour faire passer le trafic lié aux services hébergés sur ce même serveur. Il est préférable d'avoir plusieurs interfaces dédié au trafic pour augmenter le débit et permettre de la redondance.

Imaginons maintenant que nous souhaitons proposer des machines virtuelles à nos clients. A ce moment-là nous souhaiterions plutôt visé une grande quantité de ressource. De cette manière il nous sera possible d'héberger un grand nombre d'instances au sein de l'infrastructure.
- CPU : Ici nous nous concentrerons donc sur le fait d'avoir un processeur avec une certaine quantité de cœur, de telle sorte à ce qu'un nombre important de machines virtuelles puissent fonctionner en parallèle. Bien évidemment une haute fréquence sera toujours préférable cependant ce n'est pas forcément un point bloquant sauf cas particulier nécessitant une machine virtuelle avec de très hautes performances.
- RAM : Comme pour le CPU, ici le but sera d'avoir un maximum de RAM quitte à réduire la fréquence de celle-ci, toujours dans le but de maximiser le nombre d'instances pouvant être hébergé au sein de l'infrastructure
- Stockage : Contrairement au stockage des fonctions de notre cas précédent, ici nous devons stocké des systèmes d'exploitations complet ainsi que tous les éléments nécessaire pour faire fonctionner les services qui seront exécutés au sein des instances virtuelles. Encore une fois le but va être d'avoir un espace de stockage conséquent. Une solution pour réduire les coûts seraient d'avoir 2 types de stockages, l'un rapide où l'on stockerait l'OS des VM et le second plus lents mais aussi plus grand permettant de stocker les grand volumes de données. Si le budget le permet le stockage rapide sera assuré par des SSD NVMe et le lent par des SSD SATA. Dans le cas contraire, les SSD SATA feront office de stockage performant et des HDD pourront remplir le rôle de stockage de données.
- Réseau : même chose que pour la partie précédente.


# Comment gérer le stockage

La gestion du stockage au sein d'une infrastructure est un autre élément nécessitant une certaine attention. La question du stockage est loin d'être une question anodine, car elle va avoir un impact direct sur les futurs évolutions de l'infrastructure. I

## Système de fichier

## Redondance des disques
## Architecture d'accès au stockage


# Où stocker le matériel
