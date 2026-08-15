Pour toutes infrastructures informatique le choix du matériel est une question primordial. Celui-ci est dirigé par 2 éléments centraux le besoin auquel doit répondre l'infrastructure ainsi que le budget disponible. C'est d'ailleurs ce second facteur qui pose le plus souvent problème, ce qui est d'autant plus vrai avec toutes les récentes augmentations des prix des composants informatiques.

Il est important de choisir du matériel en cohérence avec les besoins définis pour l'infrastructure car un choix non adapté pourrait limiter les performances de cette dernière voir la rendre certaines fonctionnalités clés inexploitables.
C'est pourquoi il est impératif de comprendre le besoin à l'origine du projet informatique avant d'émettre n'importe quel choix.
Chaque service qu'accueillera l'infrastructure est susceptible d'avoir ses propres besoins, il est nécessaire de tous les prendre en compte et cela à chaque étape du design de l'infrastructure.

Avant de choisir le matériel il faut donc comprendre la charge principal auquel devra répondre l'infrastructure pour faire fonctionner le ou les services à héberger.

Dans notre cas, nous souhaitons mettre en place, un service d'exécution serverless. Ici la caractéristique la plus importante est la performance, le but va être de choisir un matériel permettant de réduire au maximum le temps entre le réception de la requête et l'envoi de la réponse par le serveur d'exécution. 
- CPU : Pour cela, nous allons donc privilégié des processeurs avec une haute fréquence quitte à prendre un modèle avec un peu moins de cœur (cela est a adapter en fonction de la charge estimée que devra encaisser l'infrastructure ainsi que du budget prévu pour celle-ci). Si jamais le processeur choisi est en-dessous des nécessités du service cela peut se traduire en un ralentissement global de ce dernier, car le processeur étant surchargé celui-ci mettra nécessairement plus de temps à traiter les requêtes.
- RAM : Il faut ensuite estimer la RAM nécessaire, comme nous visons la vitesse les plus hautes fréquences en DDR5 seront le mieux évidemment si le financement ne suit pas à ce moment-là la DDR4 pourra être une option de secours. Maintenant concernant la quantité de RAM en elle même, il faut réaliser une estimation du nombre. Sachant que la RAM doit augmenter en adéquation avec le nombre de cœur du processeur. La RAM est un élément centrale pour un FaaS car c'est la RAM qui est utilisé pour stocker les fonctions qui ont été récemment exécutés ainsi permettre de réduire le temps de réponse. Plus la RAM est rapide, plus la réponse le sera, plus il y a de RAM, plus il sera possible de stocker des fonctions qui seront rapidement exécutables.
-  Stockage : Concernant le stockage, vu que la vitesse est le critère recherché, nous nous orienterons donc vers des SSD NVMe si le budget le permet et dans le cas contraire des SSD SATA. Dans le cas du serverless, les données à stockées ne sont pas très lourdes, car il ne s'agit que de fichiers de codes que l'on associe à un identifiant. Cependant, ces données ont besoins d'être rapidement accessibles pour limiter l'impact du Cold Start.

# Compute

Si nous souhaitions mettre à disposition des machines virtuelles à nos clients, dans ce cas, nous aurions favoriser le choix de processeur avec un plus grand nombre de cœurs pour augmenter le pool de ressource à partager entre les différentes instances virtuelles quitte à ce que ces dernières soient un peu moins performantes à cause de la fréquence potentiellement un peu plus faible.

Si nous voulions mettre en place un service de stockage, nous aurions mis ici l'accent sur un processeur économe en ressource mais ayant tout de même un certain nombre de cœurs notamment pour les fonctions de chiffrement et de compression ainsi que de nombreuses lignes PCIe permettant ainsi une gestion d'un plus grand pool de disques.

# Storage

Dans notre cas, nous souhaitons mettre en place, un service d'exécution serverless. Ici la caractéristique la plus importante est la performance, le but va être de choisir un matériel permettant de réduire au maximum le temps entre le réception de la requête et l'envoi de la réponse par le serveur d'exécution.

Dans le cas des machines virtuelles, il est bien évidemment préférable de choisir des SSD NVMe car bien plus performant, cependant des SSD SATA pour le stockage des OS et des HDD pour le stockage de données peut être parfaitement adapté en fonction des services devant être virtualisé.

Pour un service de stockage

Pour un service de sauvegarde à froid, on s'orientera généralement vers des disques à bandes magnétiques ou encore des disques durs à basse consommation. Les 2 ne sont pas les matériels les plus performants cependant ils permettent de sauvegarder de grandes quantités de données à moindre coup. Dans le cas des bandes magnétiques, elles proposent en plus une durée de vie pouvant aller entre 20 et 30 ans contrairement au disque dur qui semble se limiter à une durée de vie allant de 5 à 10 ans pour un usage modéré.

# Localisation du matériel
