protocole de routage à vecteur de distance -> RIP ne fonctionne qu'avec la table de routage (RIB : Routing Info. Base) -> la meilleur route pour chaque dest. éventuelles les meilleurs si qualité (métrique) égales -> load balancing
Métrique : Hop count -> nombre de routeur traversés jusqu'à destination, max 15 router
Distance administrative : 120
Fonctionnement de RIP :
A---[R1]--O-[R2]-P--[R3]---D 
     |             |            |             B
Distance Administrative (AD) : valeur décimal qui représente la confiance que le router va porter en la source de l'information.
Plus la valeur est faible, plus le router fait confiance et choisi la route. Convergence lente



Delay de serialisation, le temps où la pulsion doit être à 1 ou à 0 pour être compréhensible.
1 nanosecondes pour lien Ethernet 10Gpbs.

