# GLBP
Gateway Load Balancing Protocol, Proprio Cisco
A une VIP correspond jusqu'à 4 MAC virtuelles
Algo load balancing : 
- round-robin : chaque routeur est sollicité à tour de rôle
- weighted : la charge est réparti entre les routeurs en fonction du poids qui leur est définit, le poids n'a pas d'unité de mesure. Les interfaces ayant un poids à 0 ne sont pas utilisées. On peut utiliser un track permettant de suivre l'état de fonctionnement d'un lien et décrementer le poids de l'interface GLBP si jamais le lien traqué venait à tomber. Le poids par défaut est 100
- host-dependant : 
