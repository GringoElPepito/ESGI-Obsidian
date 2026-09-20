# Les composants informatiques

Les composants informatiques sont les éléments qui une fois assemblés permettent d'aboutir à ce qu'on appelle communément un ordinateur.
Voici la liste des principaux éléments composants un ordinateur :
- Le CPU ou processeur est un composant dit polyvalent qui excelle dans la réalisation les calculs dits complexes, il traite relativement peu d'opération à la fois mais le fait à une grande vitesse.
- La RAM ou mémoire vive qui va permettre de stocker de manière temporaire les informations nécessaires aux calculs du processeur. Ce type de mémoire propose une très faible latence ce qui parfait pour accès rapide aux données stockés dans la RAM par le processeur. Les données enregistrées dans la RAM ne sont pas conservés entre les redémarrages ou lorsque la machine est éteinte.
- Le GPU ou carte graphique ou processeur graphique est le composant qui se charge de résoudre de large quantité de calculs simples (généralement des calculs matricielles), ce qui est parfaitement adapter aux calculs graphiques (affichage) ou encore à l'intelligence artificielle.
- La VRAM ou mémoire vive graphique, est généralement directement intégré à la carte graphique et rempli le même rôle que la RAM mais pour la carte graphique. Ce type de mémoire possède une très large bande passante permettant de transmettre une large quantité de données au GPU parfait pour que celui-ci réalise ces nombreux calculs simultanément. A l'instar de la RAM, la VRAM ne conserve pas non plus les données entre les redémarrage ou lorsque la machine est éteinte.
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

Maintenant nous allons aborder les critères à regarder pour chacun des composants cités plus tôt. Nous ne traiterons pas les critères de sélection pour un GPU car celui-ci n'est pas nécessaire pour notre cas.

Le premier élément est le CPU, lors de la sélection d'un processeur il y a plusieurs critères sur lesquelles s'attarder :
- Le nombre de cœurs définit le nombre de tâche que le processeur va pouvoir traiter en parallèle. Cependant la notion est un peu plus complexe que ça, il existe 2 types de cœurs, les cœurs physiques et les cœurs logiques. Les cœurs physiques sont des unités directement présentes sur le processeur, ce sont eux qui sont chargés de réaliser les calculs (ex: envoie des instructions au disque pour lecture/écriture de fichier, chiffrement de données, traitement d'une trame réseau reçu par la carte réseau etc...). Chaque cœur physique ne peut traiter qu'un seul calcul à la fois, c'est pourquoi les processeurs en possèdent plusieurs, ils sont ainsi capables de réaliser plusieurs calculs en parallèles. Pour pousser plus loin les capacités d'un processeur a réaliser du multi-tâches Intel et AMD ont réciproquement introduit les technologies Hyper-Threading et Simultaneous Multi-Threading (SMT). Pour faire simple ces technologies vont permettre à chaque cœur physique de traiter 2 listes de tâches en "même temps". Le système d'exploitation va donc voir 2 cœurs logiques pour chaque cœur physique et va donc transmettre des calculs à chacun des cœurs logiques. Les cœurs physiques vont ensuite alterner entre les 2 listes de tâches pour répondre à tous les calculs qui leurs sont transmis le plus rapidement possible. Cependant, l'activation de l'Hyper-Threading ou du SMT n'est pas gratuite, chaque cœur physique étant pleinement exploité, cela peut amener à une plus grande consommation électrique ainsi qu'une tendance plus importante à chauffer ce qui n'est pas une bonne chose pour le processeur. De plus, pour des programmes ne gérant pas forcément l'utilisation du Multi-Threading comme le jeu vidéo, cette fonctionnalité peu amenés à une baisse des performances. Cela mis à part, le Hyper-Threading/SMT convient parfaitement à notre cas d'usage qui consistera à exécuter un grand nombre de fonction en parallèle.
- La fréquence d'horloge (en GHz), cette valeur indique la vitesse à laquelle le processeur réalise les opérations qui lui sont transmises. Certains processeurs propose un mode Boost/Turbo qui permet d'augmenter cette fréquence généralement au détriment d'une consommation électrique plus élevé ainsi que d'une température qui aura tendance à augmenter.
- La mémoire cache, cette mémoire est directement intégré au processeur. Elle remplit globalement le même rôle que la RAM c'est à dire stocker les informations nécessaires au fonctionnement du processeur mais elle le fait bien plus rapidement que la RAM. Il y a 3 niveau de cache L1, L2 et L3. L1 est le plus rapide mais aussi celui avec la plus petite capacité, L3 est le plus lent mais possède la plus grande capacité et L2 est un compromis entre les deux. Globalement plus il y en a mieux c'est.
- Le socket et la compatibilité, ils sont des éléments important car ils vont déterminés la liste des choix possibles concernant la RAM et la carte mère. La partie de la carte mère sur laquelle se positionne le processeur s'appelle le socket, celui change en fonction de la marque (Intel ou AMD) et des générations de processeur. Il sera donc impossible de faire fonctionner un processeur avec une carte mère dont le socket n'est pas compatible. La compatibilité concerne la génération de RAM utilisable, nous détaillerons se point lorsque nous aborderons la RAM en elle-même, mais ce qu'il faut comprendre c'est qu'il n'est pas possible d'utiliser une barrette de RAM de 4e génération (DDR4) avec un processeur fonctionnant uniquement avec de la RAM de 5e génération (DDR5). 
- Le TDP (Thermal Design Power ou puissance thermique de conception) contrairement à ce que beaucoup de personne pense, le TDP ne représente pas la consommation du processeur mais la chaleur maximale théorique (exprimé en watts) que le système de refroidissement devra être en capacité d'évacué. Tout équipement ou matériel traversé par de l'électricité chauffe, cependant plus un processeur chauffe plus ses performances se détériore, c'est pourquoi la mesure du TDP est importante. Cette valeur est conceptuel et non absolue, dans certains cas un processeur peut être amener à relâcher plus de chaleur que la valeur indiqué par le TDP. Le TDP peut tout de même servir d'indicateur quant à la consommation d'un processeur, cependant celle-ci peut grandement varié en fonction de son utilisation et n'est donc pas à prendre au pied de la lettre. L'idéal est de prendre un processeur avec un TDP assez bas ainsi sa consommation électrique et la chaleur produite sera plus faible, le processeur aura donc moins besoin d'être refroidi pour fonctionner de manière optimale.

Carte Mère
- Le socket est le premier point d'attention à avoir lorsque vous choisissez une carte mère. Le socket est la partie de la carte mère sur laquelle on va venir positionner le processeur. C'est cet élément qui va définir quels processeurs pourront être utilisés, à noter qu'on choisit généralement le processeur puis une carte mère adapté à celui-ci.
- Le chipset est l'élément centrale du carte mère, c'est lui qui permet la communication entre processeur et les autres composants de l'ordinateurs. Le chipset définit le nombre d'équipement qui pourront être branché simultanément (Carte Graphique, SSD, HDD etc...) ou encore les types de connectiques que la carte mère pourra proposer. Il peut aussi être un facteur limitant quant à la vitesse maximale à laquelle pourra fonctionner la RAM. Le chipset à une influence direct sur le prix de la carte mère car il définit la gamme du produit, il est donc important de connaître ses besoins pour choisir la carte mère la plus adapté et ainsi payer uniquement ce qui convient à vos besoins.
- Le format correspond à la taille de la carte mère, généralement plus une carte mère est grande plus elle proposera de connectiques. Encore une fois choisissez en fonction de vos besoins. Voici les formats sont les suivants du plus petit au plus grand :
	- mini-ITX
	- micro-ATX
	- ATX
	- Extended-ATX
- Le nombre slots de RAM et de Stockage, ce dernier est la plupart du temps lié au Chipset et au format choisi. Ce point peut être un facteur limitant concernant l'évolution de votre matériel. Si vous prenez une carte mère n'ayant que 2 slots de RAM et que ces derniers sont déjà utilisés alors vous serez obligés de remplacer vos barrettes pour augmenter la quantité de RAM, cependant si votre carte mère en possède 4 alors à ce moment-là vous pouvez simplement acheter de la RAM supplémentaire et l'ajouté directement aux côtés de celles déjà présente. C'est la même chose pour le stockage, il est donc important de prendre compte vos besoins ainsi que leurs potentielles évolutions.

RAM 
- Capacité (en Go), c'est l'espace disponible qui sera exploitable par le processeur pour stocker les informations dont il a besoin. Idéalement on adapte la quantité de RAM en fonction de la charge de travail auquel devra répondre l'ordinateur, c'est donc à vous de vous renseigner en fonction de vos besoins. 
- Fréquence (en MHz), c'est la vitesse des échanges de données proposée par la RAM, généralement plus cette valeur est élevé mieux ce sera. Cependant, il est important de vérifier la fréquence maximal de la RAM prise en charge par le CPU et la carte mère, car si celle-ci est inférieure à la fréquence proposée par la RAM alors la RAM devra être bridé pour fonctionner à la vitesse supportée. De cette manière la RAM ne pourra pas être pleinement exploité ce qui est assez dommage au vu de son prix actuel.
- Compatibilité, Pour faire simple la version de la RAM est généralement présenté avec la présence de la mention DDRX où DDR signifie Double Data Rate et le X correspond à la version. La technologie DDR permet de transférer des données deux par cycle d'horloge ce qui permet de doubler les performances par rapport à du Single Data Rate sans augmenter la fréquence des cycles. La dernière version est là version 5 (DDR5). Les principales différences entre les générations sont l'augmentation du débit de données, la réduction de la consommation électrique et une globalement une meilleure efficacité d'une génération à l'autre. Chaque génération est physiquement différente, en effet les barrette de RAM possède une encoche dont la position varie entre les générations rendant impossible l'utilisation d'une barrette de DDR3 sur une carte mère supportant la DDR2 ou la DDR4. 
- Mode Dual Channel, c'est une technologie permettant de doubler la bande passante de la RAM en exploitant deux canaux simultanément. Cette technologie nécessite deux barrettes de RAM pour être utilisé.
- Latence (CAS pour Column Address/Access Strobe) est exprimé avec une valeur numérique absolue (CL16) permet de mesurer le temps que met la RAM à répondre lorsqu'elle est sollicité. Plus la valeur est basse plus RAM répondra vite.

Stockage
- Le support de stockage, Il en existe de plusieurs sortes :
	- Les SSD utilise la mémoire flash pour stocker les données, l'entièreté des données est stockés de manières électroniques à l'aide d'électrons piégés ou non dans des transistors représentant ainsi les 1 et les 0 du langages binaire et cela même sans alimentation.
		- Les SSD NVMe sont à ce jour la version de la mémoire morte la plus performante mais par la même occasion la plus onéreuse. Ces composants se connecte directement à la carte mère via les ports PCI Express (ou PCIe) ce qui permet des débit entre 5 à 10 fois supérieurs (dépendant de la génération PCIe) aux équipements les plus rapides fonctionnant en SATA. Il existe différente génération de SSD NVMe basé sur les génération de port PCIe, la dernière version est actuellement la version 5. La différence principale entre les générations et bien évidemment la performance qui augmente d'une génération à l'autre.
		- Les SSD SATA sont une version plus anciennes des SSD NVMe utilisant un câble SATA pour se connecter à la carte mère. Bien que moins rapide que la version NVMe, ils restent tout de même assez intéressant car ils sont un bon compromis entre le prix, la vitesse et la taille du stockage.
	- Les HDD utilise plusieurs disques magnétiques pour stocker les informations, une tête de lecture/écriture parcourt ces disques pour lire ou écrire des données. Les données sont inscrites à l'aide de d'impulsion électriques servant à aimanté des petites zones sur le disques de manière à représenter un 1 ou un 0. Le HDD est la manière la plus lente de toutes celles présentées pour stocker des données. Cependant c'est aussi la moins chère.
- Les vitesses de lectures et d'écritures (en Mo/s), définissent les débit de données maximums en entrées (écritures) et en sorties (lecture). Comme expliqué plus tôt le premier facteur qui influencera la vitesse, ça sera le support physique choisit les SSD NVMe étant les plus rapides et les HDD les plus lents. Cependant tous les SSD NVMe ne se valent pas tous notamment entre les différentes générations qui peuvent avoir des différences de vitesse pouvant aller jusqu'à 6 fois plus vite si on compare la dernière génération avec la première.
- La capacité (en To ou Go) correspond à la quantité de données qui pourra être stocké au sein de l'équipement, le but étant de réduire les coûts dimensionner en fonction de vos besoins. A prix égal un HDD propose généralement une bien plus grande capacité qu'un SSD SATA ou NVMe

Alimentation
- La puissance (en Watt) est la quantité d'électricité que l'alimentation va pouvoir fournir, pour ce critère, il faut d'abord avoir choisi tous les autres composants car ce sont eux qui vont définir l'énergie nécessaire à transmettre pour que l'ordinateur puisse fonctionner.
- Le rendement correspond à la faculté d'une alimentation à limiter les pertes d'énergie sous forme de chaleur lors de la conversion du courant alternatif en courant continu. Ce rendement se mesure avec les certifications 80+, voici les certifications existantes du moins bon au meilleur rendement :
	- 80+
	- 80+ bronze
	- 80+ silver
	- 80+ gold
	- 80+ platinum
	- 80+ titanium
- La modularité est la capacité d'une alimentation à pouvoir débrancher ou non les câbles servant à faire transité l'électricité de l'alimentation jusqu'aux composants. Si certains câbles ne peuvent être retirés, alors ils devront rester dans le boitier ce qui peut amener à une mauvaise circulation de l'air au sein de celui-ci conduisant à un moins bon refroidissement de la machine.

Prenons le cas de ce cours c'est à dire la mise en place d'un service d'exécution serverless. Ici la caractéristique la plus importante est la performance, le but va être de choisir un matériel permettant de réduire au maximum le temps entre le réception de la requête et l'envoi de la réponse par le serveur d'exécution. 
- CPU : Pour cela, nous allons donc privilégié des processeurs avec une haute fréquence quitte à prendre un modèle avec un peu moins de cœur (cela est a adapter en fonction de la charge estimée que devra encaisser l'infrastructure ainsi que du budget prévu pour celle-ci). Si jamais le processeur choisi est en-dessous des nécessités du service cela peut se traduire en un ralentissement global de ce dernier, car le processeur étant surchargé celui-ci mettra nécessairement plus de temps à traiter les requêtes.
- RAM : Il faut ensuite estimer la RAM nécessaire, comme nous visons la vitesse les plus hautes fréquences en DDR5 seront le mieux évidemment si le financement ne suit pas à ce moment-là la DDR4 pourra être une option de secours. Maintenant concernant la quantité de RAM en elle même, il faut réaliser une estimation de la quantité nécessaire. Sachant que la RAM doit augmenter en adéquation avec le nombre de cœur du processeur. La RAM est un élément centrale pour un FaaS car c'est la RAM qui est utilisé pour stocker les fonctions qui ont été récemment exécutés pour ainsi permettre de réduire le temps de réponse. Plus la RAM est rapide, plus la réponse le sera, plus il y a de RAM, plus il sera possible de stocker des fonctions qui seront rapidement exécutables.
- Stockage : Concernant le stockage, vu que la vitesse est le critère recherché, nous nous orienterons donc vers des SSD NVMe si le budget le permet et dans le cas contraire des SSD SATA. Dans le cas du serverless, les données à stockées ne sont pas très lourdes, car il ne s'agit que de fichiers de codes que l'on associe à un identifiant. Cependant, ces données ont besoin d'être rapidement accessibles pour limiter l'impact du Cold Start.
- Réseau : l'objectif ici va être d'avoir la plus grosse bande passante possible, la première. On essaiera si possible pour la majorité des serveurs d'avoir au minimum 2 cartes réseau, la première servant au management du serveur, la seconde pour faire passer le trafic lié aux services hébergés sur ce même serveur. Il est préférable d'avoir plusieurs interfaces dédié au trafic pour augmenter le débit et permettre de la redondance.

Imaginons maintenant que nous souhaitons proposer des machines virtuelles à nos clients. A ce moment-là nous souhaiterions plutôt visé une grande quantité de ressource. De cette manière il nous sera possible d'héberger un grand nombre d'instances au sein de l'infrastructure.
- CPU : Ici nous nous concentrerons donc sur le fait d'avoir un processeur avec une certaine quantité de cœur, de telle sorte à ce qu'un nombre important de machines virtuelles puissent fonctionner en parallèle. Bien évidemment une haute fréquence sera toujours préférable cependant ce n'est pas forcément un point bloquant sauf cas particulier nécessitant une machine virtuelle avec de très hautes performances.
- RAM : Comme pour le CPU, ici le but sera d'avoir un maximum de RAM quitte à réduire la fréquence de celle-ci, toujours dans le but de maximiser le nombre d'instances pouvant être hébergé au sein de l'infrastructure
- Stockage : Contrairement au stockage des fonctions de notre cas précédent, ici nous devons stocké des systèmes d'exploitations complet ainsi que tous les éléments nécessaire pour faire fonctionner les services qui seront exécutés au sein des instances virtuelles. Encore une fois le but va être d'avoir un espace de stockage conséquent. Une solution pour réduire les coûts seraient d'avoir 2 types de stockages, l'un rapide où l'on stockerait l'OS des VM et le second plus lents mais aussi plus grand permettant de stocker les grand volumes de données. Si le budget le permet le stockage rapide sera assuré par des SSD NVMe et le lent par des SSD SATA. Dans le cas contraire, les SSD SATA feront office de stockage performant et des HDD pourront remplir le rôle de stockage de données.
- Réseau : même chose que pour la partie précédente.

Les 2 cas présentés ci-dessus sont des exemples pouvant servir de base pour vos futurs décisions architecturales. L'important est de comprendre le besoin et de définir les éléments qui seront les plus à même d'y répondre.
# Comment gérer le stockage ?

La gestion du stockage au sein d'une infrastructure est un autre élément nécessitant une certaine attention. La question du stockage est loin d'être une question anodine, car elle va avoir un impact direct sur les futurs évolutions de l'infrastructure. 
Il y a quatre points sur lesquelles s'arrêter pour définir une politique général concernant le stockage :
- Redondance des disques
- Architecture d'accès au stockage
- Mode de stockage
- Stockage en mode bloc
- Stockage en mode fichier
- Stockage en mode objet
- Sauvegarde
## Redondance des disques

Le stockage repose sur des équipements physiques qui peuvent donc de facto tomber en panne. Pour palier cela il existe la solution RAID (Redundant Array of Independent Disks) qui consiste à associer plusieurs disques dans le but d'améliorer les performances, la redondance ou les deux.

Voici les types de RAID basiques :
- RAID 0 (ou stripping) : Ici on va combiner 2 disques ou plus pour que toutes les données inscrites soient réparties entre les disques ce qui permet d'améliorer les performances. Cependant si l'un des disques tombe en panne l'entièreté des données sont perdues. Le RAID 0 permet d'exploiter pleinement l'espace disponible sur chaque disque. Ce RAID est celui présentant les meilleurs performances en lecture et en écriture.
- RAID 1 (ou mirroring) : Requiert 2 disques ou plus. Ici on va simplement dupliquer les données sur l'ensemble des disques présent au sein du RAID. De cette manière, si l'un des disques venaient à tomber en panne, les autres seraient immédiatement utilisable. L'espace disponible pour le RAID 1 correspond à la taille du plus petit disque du RAID. Il est donc assez peu intéressant d'utiliser plus de 2 disques pour un RAID 1.
- RAID 5 : Requiert 3 disques ou plus. Ce RAID fonctionne avec des blocs de parités distribués entre les disques du RAID qui permettent de reconstruire les données perdus en cas de panne d'un des disques. Ce RAID permet de supporter la panne d'un seul et unique disque. L'espace disponible pour le RAID 5 se calcul de la manière suivante : k * ( n - 1 ), où k représente la taille du plus petit disque et n le nombre de disque disponible au sein du RAID. L'ajout des blocs de parités peut ralentir la performance d'écriture. 
- RAID 6 : Requiert 4 disques ou plus. Ce RAID fonctionne lui aussi avec des blocs de parités cependant, chaque bloc de parité est dupliqué sur un second disque. Ce RAID permet de supporter jusqu'à deux pannes de disques simultanés. L'espace disponible pour le RAID 6 se calcul de la manière suivante : k * ( n - 2 ), où k représente la taille du plus petit disque et n le nombre de disque disponible au sein du RAID. L'ajout des blocs de parités peut ralentir la performance d'écriture et cela davantage que pour le RAID 5 car il a 2 blocs de parités pour chaque bloque de données. 

Les RAID 2, 3 et 4 existent aussi cependant ces derniers ne sont que très rarement voir même jamais utilisé car ils sont globalement moins efficace que ceux cités plus tôt.

Concernant la réalisation d'un RAID quel qu'il soit, il est préférable d'utiliser uniquement des disques de la même taille et de la même vitesse. Dans le cas contraire, l'espace disponible sera basé sur le disque avec la taille la plus faible, exemple : un RAID 0 avec un disque de 500Go et un autre disque de 1To ne fournira qu'un espace de stockage de 1To (500Go pour le premier disque et 500Go pour le second disque), ce qui représente une perte sèche de 500Go dans ce cas. Concernant la vitesse similaire entre les disques, cela est surtout dans le but d'obtenir des performances stables et prévisibles ce qui pourrait ne pas être le cas avec des disques ayant des vitesses différentes.

Il est possible de combiner plusieurs types de RAID entre eux généralement dans le but de profiter des performances et de la sécurité offert par les types combinés
Voici maintenant les RAID imbriqués :
- RAID 10 : Requiert minimum 4 disques. On va mettre en place plusieurs paires en RAID 1 chaque paire sera ensuite combiné avec les autres au sein d'un RAID 0. A la fois performant et sécurisé au détriment de l'espace disponible.
- RAID 01 : Requiert minimum 4 disques. C'est l'inverse du RAID 10, on va créer des paires en RAID 0 qui seront ensuite associé pour former un RAID 1. On préférera généralement le RAID 10 car plus résilient. Si un disque tombe au sein d'un RAID 01 alors la paire concernée doit être entièrement reconstruite.
- RAID 50 : Requiert minimum 6 disques. On va réaliser plusieurs RAID 5 qui seront ensuite agrégés dans un RAID 0. Ce qui permet un certain niveau de sécurité et de performance sans perdre trop d'espace disponible. Ce RAID tolère 1 panne par groupe RAID 5. 
- RAID 60 : Requiert minimum 8 disques. On va réaliser plusieurs RAID 6 qui seront ensuite agrégés dans un RAID 0. Ce RAID fournit un niveau de sécurité supérieur au RAID 50 au détriment d'une perte d'espace disponible.

Voici un tableau comparatif des différents RAID présentés ci-dessus :
- k représente la taille du plus petit disque
- n représente le nombre du disque
- m représente le nombre de groupe

| Type    | Disques min. | Espace Dispo. | Tolérance panne     | Lecture    | Ecriture                   |
| ------- | ------------ | ------------- | ------------------- | ---------- | -------------------------- |
| RAID 0  | 2            | k * n         | Aucune              | Excellente | Excellente                 |
| RAID 1  | 2            | k * n / 2     | 1 disque            | Bonne      | Moyenne                    |
| RAID 5  | 3            | k * (n - 1)   | 1 disque            | Bonne      | Moyenne (parité)           |
| RAID 6  | 4            | k * (n - 2)   | 2 disque            | Bonne      | Plus lente (double parité) |
| RAID 10 | 4            | k * n / 2     | 1 par paire RAID 1  | Excellente | Excellente                 |
| RAID 01 | 4            | k * n / 2     | 1 par groupe        | Excellente | Excellente                 |
| RAID 50 | 6            | k * (n - m)   | 1 par groupe RAID 5 | Très Bonne | Bonne                      |
| RAID 60 | 8            | k * (n - 2m)  | 2 par groupe RAID 6 | Très Bonne | Correcte                   |

Maintenant que vous connaissez les forces et faiblesses de chaque type de RAID, il faut maintenant choisir le celui qui conviendra à notre infrastructure.

Dans le cas de notre service d'exécution serverless, il est important de savoir ce que va réaliser la majorité des fonctions qui vont être exécutés. Si les fonctions ne font pas d'écriture sur le disque alors un RAID 50 ou 60 dépendant du niveau de sécurité voulu pourrait parfaitement faire l'affaire. Cependant si les fonctions réalisent un grand nombre d'écriture alors il serait préférable d'utiliser un RAID 10 quitte à perdre de l'espace. Bien évidemment le plus important est comme toujours de parvenir à concilier le budget avec les besoins de l'infrastructure.

### Architecture d'accès au stockage

L'architecture de stockage correspond à la manière de connecter les stockages au serveurs de calcul. Voici les 5 architectures de stockage les plus courantes :
- DAS (Direct Attached Storage), ici les disques servant d'espaces de stockages sont directement connectés aux machines qui pourront donc localement exploiter ces derniers.
- NAS (Network Attached Storage), les machines vont ici se connecter à un serveur de stockage distant en passant à travers le réseau physique global de l'infrastructure.
- SAN (Storage Area Network), les machines vont se connecter à un ou plusieurs serveurs de stockage à travers un réseau physique dédié (Switch, Câbles etc..) servant à transporter uniquement le trafic entre les machines de calculs et les serveurs de stockages.
- HCI (HyperConverged Infrastructure), comme pour le DAS, les disques sont directement connectés sur chacune des machines, cependant, chaque machine va mettre en commun son espace de stockage avec les autres pour former un stockage unifié entre toutes les machines du cluster. L'interconnexion entre les machines peut se faire via le réseau physique global ou de préférence via un réseau physique dédié pour de meilleur performance.
- Cloud, les machines sortent sur internet dans le but d'atteindre le stockage distant fournit par un Cloud Provider (AWS, Azure, GCP ou autre), le stockage est donc externe à l'infrastructure On-Premise.

### Mode de stockage
Le mode de stockage correspond à la manière dont les machines vont interagir avec l'espace de stockage pour .
Il existe 3 mode d'accès :
- Stockage en mode bloc
- Stockage en mode fichier
- Stockage en mode objet

Stockage en mode bloc, toutes les données vont être découpés en paquets de taille fixe (bloc) possédant chacun sa propre adresse, permettant ainsi de modifier un bloc spécifique sans avoir à modifier entièrement le fichier qui y est lié. C'est le mode le plus bas niveau (proche de la machine) mais aussi le plus performant. 
Les hautes performances et la faible latence sont par ailleurs ses avantages principaux, qui sont entres autres permis par la possibilité de modifier uniquement les blocs voulus et non le fichier en entier. 
Un inconvénient majeur est qu'un espace de stockage par bloc est par défaut utilisable par une seule et unique instance. Il existe malgré tout des protocoles de stockage par bloc ou des systèmes de fichiers intègrent des systèmes permettant un accès multiple à un espace de stockage par bloc unique, cela peut rendre la mise en place assez complexe. 
Ce type de stockage excelle pour le stockage de Machine Virtuelle ou pour les base de données transactionnelles comme MySQL, PostgreSQL ou encore Oracle SQL.

Stockage en mode fichier, les données sont ici directement représentés sous forme de fichier organisé en hiérarchie de dossiers. C'est le mode de stockage utilisé par les systèmes d'exploitations, il vient généralement se placer au dessus du stockage en mode bloc pour faciliter l'exploitation du stockage par un humain. 
Ses principaux avantages sont les suivants :
- La possibilité de mettre en place un contrôle d'accès ainsi que du partage de fichier
- Utilisable par plusieurs utilisateurs ou instance simultanément
Cependant, il compte aussi certains inconvénient :
- Plus le volume de données est grand plus les performances ont tendances à baisser
- Il peut être assez complexe de mettre à l'échelle un stockage en mode fichier surtout si les besoins évolue rapidement
Le stockage en mode fichier excelle particulièrement pour la collaboration, le partage de document ainsi que pour les accès simultanés par plusieurs instances.

Stockage en mode objet, avec ce mode de stockage il n'y a aucun dossier ni aucune arborescence ou hiérarchie. Toutes les données sont rassemblés en unités indépendantes les unes des autres que l'on appelle objets. Comme il n'y a pas de dossier l'ensemble des objets se retrouvent au même niveau. Chaque objet contient ses données, ses métadonnées et un identifiant unique (qui fait partie des métadonnées). L'accès aux objets se fait généralement à travers des requêtes web ou API.
Les avantages du stockage en mode objet sont les suivants :
- Mise à l'échelle simplifié
- Offres Cloud à des prix compétitifs
- Métadonnées illimitées
Les inconvénients sont les suivants :
- Modifications fastidieuses (Il faut écraser la version existante d'un fichier avec la nouvelle pour le modifier)
- La latence peut être assez élevé comparé aux autres mode de stockage
Le stockage en mode objet est très adapté au stockage de données d'archivage, de sauvegarde, de site web statique, de grands volumes de données non structurées (Vidéo, Image etc...), Big data, IA, Analyse de données.

Ces 3 modes sont combinables et répondent à des besoins distincts. Prenons une machine avec un HDD servant à stocker des films qui pourront être par la suite diffusé sur un site internet. Pour écrire les données sur le disque, la machine utilisera le stockage en mode bloc, car c'est le seul moyen de communication direct avec le disque. Cependant pour agrégé les blocs sous forme de fichiers lisibles, on utilisera le stockage en mode fichier à travers un système de fichier qui pourra faire la traduction de fichier à bloc et inversement. Enfin pour rendre accessible par notre application les films stockés sur le disque on pourra utiliser un stockage en mode objet qui facilitera la récupération des films pour l'application.

## Système de stockage par bloc
Comme expliqué précédemment le stockage par bloc permet de lire et d'écrire sur un espace de stockage local ou distant en accédant directement aux blocs constituants les données. On peut regroupé les système de stockage par bloc dans 2 catégories les systèmes locaux et les systèmes en réseaux
Voici les systèmes de stockage par bloc locaux :
- SCSI (Small Computer System Interface) est un standard d'interface matérielle et un protocole visant à simplifier l'envoie d'instruction au périphérique de stockage par le système d'exploitation. SCSI a été initialement conçu pour les disques durs, cependant l'apparition des mémoires flash a poussé l'intégration d'instructions adapté à ce nouveau type de mémoire au sein du protocole SCSI, mais son design historique ne permet malheureusement pas d'exploité pleinement le potentiel des SSD. Dans le cas d'un disque branché en SATA, SCSI n'est qu'un intermédiaire et ne permet pas d'écrire directement sur le disque. En effet, les instructions SCSI sont par la suite traduit par SATL (SCSI-to-ATA Translation Layer) en instructions ATA qui sont elles compréhensibles par le contrôleur du périphérique de stockage et qui pourra donc les exécuter. Cependant il existe un autre type de port appeler SAS qui est assez proche de l'apparence du SATA à la différence que la partie du port servant à l'alimentation et la partie servant au transfert des données sont jointes contrairement au SATA. Dans le cas du SAS, SCSI n'est pas traduit mais directement exécuté ce qui réduit grandement la latence et augmente donc par la même occasion les performances.
- NVMe (Non-Volatile Memory Express) est à la fois une interface matérielle et un protocole de communication dédié aux échanges entre les périphériques de mémoire flash (SSD) et le reste du système via le bus PCIe (PCI Express). Etant construit précisément dans le but de fonctionner avec des SSD, NVMe possède de bien meilleur performance que SCSI pour ce type de périphérique.

SCSI et NVMe sont tous les deux très efficaces mais ils ne sont malheureusement utilisables que sur des périphériques directement branché à la carte mère du serveur à travers un câble SAS/SATA ou un port NVMe. C'est pourquoi plusieurs systèmes de stockage par bloc en réseau ont fait leur apparition. Voici les plus populaires :
- iSCSI (internet Small Computer System Interface) directement adapté de SCSI, iSCSI permet à une machine d'atteindre des disques d'un serveur distant et de transmettre des instruction SCSI en se basant sur le protocole TCP/IP. Au sein d'iSCSI, le client est ce qu'on  appelle l'initiateur (initiator) qui est celui initialisant la connexion auprès du serveur qu'on appelle la cible (target). La cible elle expose des disques virtuels qu'on appellent LUN (Logical Unit Number) qui correspond globalement à un volume ou un fragment d'un disque physique. iSCSI fonctionnant au-dessus du protocole TCP/IP il peut être déployé en architecture NAS sur un réseau physique existant sans nécessité de matériel dédié. Cependant il est tout à fait possible de mettre en place un réseau physique à part notamment pour séparer le trafic iSCSI du reste de l'infrastructure.
- NVMe over TCP, directement adapté de NVMe, NVMe over TCP présente énormément de similitude avec iSCSI. Il permet à une machine client de transférer des instructions NVMe à travers le protocole TCP/IP. Il reprend aussi les concepts, d'initiateur, de cible et de LUN que l'on a vu avec iSCSI
- FC (Fiber Channel) est un protocol de communication comme Ethernet. Fiber Channel est conçu pour des transferts de données à très haut débit, sans pertes avec un maintien de l'ordre des paquets. Il existe différentes architectures pour le Fiber Channel, cependant nous nous concentrerons sur la plus commune à savoir l'architecture Fabric (switchée ou commutée). Dans le cas de l'architecture Fabric, on va placer un ou plusieurs switch Fiber Channel qui auront pour rôle de connecter les clients aux serveurs généralement au travers de liens fibre optique. Les switches utilisés sont de type Fiber Channel et non Ethernet, ce qui signifie qu'uniquement le trafic Fiber Channel peut y transiter. Aussi il n'est pas possible d'utiliser un switch Ethernet pour mettre en place du Fiber Channel natif ce qui signifie que FC, dans un cas d'architecture Fabric, nécessite obligatoirement la mise en place d'un réseau physique dédié (SAN). Nativement, Fiber Channel transmet des instructions SCSI cependant il est possible d'y faire transiter des instructions NVMe pour obtenir du NVMe over FC. FC permet d'atteindre des performances bien supérieur aux protocoles reposant sur le protocole TCP/IP cependant il est aussi bien plus onéreux et peut aussi ajouter une certaine complexité à l'infrastructure globale.
- FCoE (Fiber Channel over Ethernet)
- NVMe-oF (Non-Volatile Memory Express over Fabric)
	- NVMe over FC
	- NVMe over TCP
	- NVMe over RDMA

## Système de fichiers 

Un système de fichier correspond au moyen utiliser pour enregistrer, structurer, nommer et indexer les données sur un support de stockage (HDD, SSD SATA/NVMe, Clé USB etc...).

Les supports physique sont en eux-mêmes assez peu exploitable, les systèmes de fichiers viennent faciliter leur utilisation en apportant une couche d'abstraction simplifiant l'écriture et la lecture des données.
### Système de fichiers locaux

Les systèmes de fichiers locaux sont utilisé pour gérer le stockage d'une machine elle-même. Ces généralement eux qui sont chargés d'inscrire les données en réalisant des appels au noyau du système voir directement au pilote des équipements de stockage. il en existe une pléthore cependant nous nous concentrerons sur les plus utilisés, en voici la liste :
- EXT4 : Ext4 (Fourth extended file system) est la dernière version disponible à ce jour des systèmes de fichiers EXT. C'est par ailleurs le système de fichiers par défaut sur Debian et Ubuntu. C'est un système par journalisation, c'est à dire qu'avant chaque modification un enregistrement de la modification a réalisé est effectué au sein de ce journal. C'est seulement après ça que la modification est appliquée. Cela permet de sécurisé les données en cas de panne système ou de coupure de courant vanne arrêté subitement la machine. Mais ce n'est pas tout, ext4 réalise aussi des allocations différée, il va garder en mémoire les modifications à réaliser sans les effectuées jusqu'à atteindre une certaine quantité de données à écrire. De cette manière, ext4 réduit la fragmentation des fichiers et réduit les coûts et le temps d'écriture. EXT4 brille dans les cas d'utilisation général, il convient parfaitement aux environnements de travail classique.

- XFS : XFS est le système de fichier par défaut pour les distributions basé sur RHEL. XFS est aussi un système de fichier par journalisation comme EXT4, il partage aussi d'autre similitude comme l'utilisation d'extents. La grande différence entre XFS et EXT4 est que XFS est optimisé pour les larges volumes de données pouvant prendre en charge des espaces de stockages allant jusqu'à 16 Exabytes de données. XFS est conçu pour faciliter l'évolutivité ainsi que le parallélisme pour notamment gérer plus opérations d'écritures/lectures simultanément. XFS propose le mécanisme Direct I/O qui permet d'outrepasser le cache de fichier kernel pour aller du cache utilisateur vers le matériel sans passer par un intermédiaire réduisant ainsi la charge CPU et augmentant directement les performances d'écriture et de lecture pour larges ensemble de données. XFS est plutôt adapté aux grandes bases de données ou stockages de fichiers ou de media volumineux.

- ZFS : ZFS est un système de fichiers propriétaire initialement développé par Sun Microsystems puis récupéré par Oracle. Il n'est pas intégré au noyau linux et nécessite de passer par l'implémentation OpenZFS pour être utilisé sur Linux. ZFS est un système de fichiers un peu particulier car il est à la fois un gestionnaire de volume comme LVM ainsi qu'un système de fichiers. ZFS est très différents des deux systèmes de fichier cités précédemment. Pour commencer, c'est un système de fichier en Copy On Write, c'est à dire que lorsqu'il y a une modification de fichier, ZFS ne va pas directement écrire par dessus l'existant mais va à la place écrire les nouvelles données au sein d'un espace distinct. Mais ce n'est pas tout, là XFS et EXT4 gère chaque disque de manière distincte, ZFS va quant à lui utiliser tous les disques disponibles pour mettre en place un pool de stockage, il est possible après cela de créer différent volumes possédant leurs propres système de fichiers. Lorsqu'un disque est ajouté au pool de stockage celui-ci est directement utilisable par les volumes existant pour y écrire des données. ZFS propose aussi nativement l'utilisation de checksums pour éviter la corruption de fichiers, de snapshots des systèmes de fichiers, RAID-Z pour la réalisation de RAID réimplémenter pour fonctionner de manière optimiser avec ZFS ainsi que le système Scrub qui permet de faire de la correction de données en erreur de manière silencieuse. Cependant toutes ces fonctionnalités ont un coup qui est une consommation de RAM et de CPU supérieur aux autres systèmes de fichiers. ZFS est excellent lorsqu'il s'agit de stockage de données critique ou encore pour la réalisation de sauvegarde.

- BTRFS : BTRFS est conçu pour être une réponse Open Source à ZFS dans le but d'être intégré au noyau Linux. BTRFS est aussi un système de fichier en Copy On Write comme ZFS, il utilise lui aussi un système de pool de stockage qui va par la suite pouvoir accueillir des volumes. BTRFS permet aussi de prendre des Snapshots des volumes qui peuvent être conservé en lecture seule ou en lecture écriture. BTRFS propose aussi une compression de fichier transparente réaliser en arrière plan ainsi qu'une implémentation des solutions RAID pour préserver l'intégrité des données. BTRFS est par la même occasion optimisé pour fonctionner sur des SSD contrairement aux autres systèmes de fichiers. Il supporte par ailleurs nativement OverlayFS qui est par exemple utilisé par Docker et Podman. De plus, BTRFS nécessite bien moins de RAM que ZFS pour fonctionner. Cependant BTRFS est encore une technologie assez jeune et compte encore un certain nombre de fonctionnalité en développement. BTRFS est un système de fichiers flexible qui profitera aux systèmes pouvant exploiter les snapshots et autres mécanismes de BTRFS comme Docker/Podman.

- VMFS : Exclusif aux solutions VMWare ESXi et développé par Broadcom VMWare. VMFS est un système de fichier par cluster qui a été entièrement conçu pour la gestion des accès concurrent par plusieurs hôtes ainsi que pour le stockage de machines virtuelles. Il peut aussi être utiliser pour stocker des fichiers plus classiques tel que des ISO mais ce n'est clairement pas ce pourquoi il a été conçu. 

- NTFS : Exclusif aux solutions Windows et développé par Microsoft. NTFS est le système de fichier par défaut sur Windows. Il permet une gestion des droits assez poussée, il possède quelques autres fonctionnalités intéressantes comme la journalisation, la compression ou encore le chiffrement. Son plus grand défaut est sa compatibilité avec des systèmes autres que Windows qui est assez restreinte car NTFS est une solution propriétaire ce qui rend difficile son intégration à d'autres systèmes. C'est le système de fichiers à utiliser pour installer l'OS de vos machines Windows.

- ReFS : Exclusif aux solutions Windows et développé par Microsoft. ReFS est un autre système de fichier de Microsoft qui est optimisé pour le stockage de grandes quantité de données critiques. Il propose plusieurs fonctionnalités de sécurité avec des checksums, une réparation automatique des données ainsi qu'une séparation des données et des métadonnées pour une meilleure résistance à une potentielle corruption des données. ReFS est aussi taillé pour la haute disponibilité et s'adapte très bien aux environnements en cluster. Un point à noter est qu'il ne prend pas en charge les partitions de démarrage, il n'est donc pas possible d'utiliser ReFS pour installer l'OS de vos Windows Serveur. Ce système de fichier est adapté pour le données critiques et/ou volumineuses, pour le stockage de machines virtuelles Hyper-V.

### Système de fichiers réseaux et distribués

Les système de fichiers réseaux et distribués ont eux un autre rôle, il s'occupe de rendre le stockage local d'une machine accessible et utilisable par d'autres machines à travers le réseau. Voici la liste de ceux utilisés en entreprise :

- NFS : NFS est le système de fichiers réseau standard pour les systèmes Linux et Unix. Via NFS les machines distantes accédant au partage le font comme s'elles accédaient à un stockage locale. Avec la version 4 de NFS (NFSv4) il est possible de restreindre l'accès du partage à travers une authentification Kerberos et des ACL. Les données sont aussi chiffrés pour garantir un niveau de sécurité maximal. Bien que développé pour les systèmes Linux et Unix, Windows possède une intégration NFS qui doit être activé dans les paramètres, cependant ce client ne supporte que la version 3 de NFS.

- SMB : SMB est un système de fichiers réseau développer par Microsoft et intégré nativement aux solutions Windows et Windows Server. La dernière version SMBv3 intègre un chiffrement bout-en-bout, SMB Multichannel qui a pour but d'agréger plusieurs connexions en parallèle ce qui améliore le débit et permet de la tolérance de panne. SMBv3 propose aussi SMB Direct qui se base sur RDMA pour augmenter le débit de transfert, réduire la latence ainsi que la charge processeur. Bien que ce soit une solution Microsoft, le noyau Linux intègre deux modules l'un prenant en charge la partie client de SMBv3 et l'autre la partie serveur.

- CephFS : CephFS est un système de fichiers distribué faisant partie de Ceph une plateforme de stockage open source. CephFS permet à plusieurs machines de partager et de modifier les mêmes fichiers simultanément tout en offrant de très haute performance et incluant de la tolérance de panne. Il fournit une interface POSIX et permet donc d'être monté via le noyau Linux. Il est par ailleurs aussi possible de monté par dessus CephFS un partage NFS ou SMB.

## Accès au stockage
Nous avons vu comment gérer les disques et stockés des données dessus. Cependant, il faut maintenant voir comment il est possible de rendre accessible cet espace de stockage à nos serveurs de calculs.






## Sauvegarde


# Où stocker le matériel

# Automatisation

# Instant Sécu

- Secure Boot et TPM
- Sécurisation des accès physique (salle serveur)
