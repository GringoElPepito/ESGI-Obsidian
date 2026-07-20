L'automatisation joue un rôle essentiel dans la gestion des infrastructures informatiques modernes en permettant d'exécuter des tâches de manière rapide, fiable et reproductible. Elle réduit les interventions manuelles, limite les risques d'erreurs humaines et améliore la cohérence des configurations sur l'ensemble des systèmes. De plus, l'automatisation facilite le déploiement des services, l'application des mises à jour et la mise en œuvre des politiques de sécurité, tout en réduisant les temps d'intervention. Dans un contexte où les infrastructures sont de plus en plus complexes, elle constitue un levier majeur pour renforcer la disponibilité, la sécurité et l'efficacité opérationnelle des systèmes d'information.

Voici les caractéristiques de fty-laut01.cenexis.lan :

| Paramètres      | Valeur                      |
| --------------- | --------------------------- |
| Type d'instance | LXC                         |
| OS              | Rocky 10.2                  |
| CPU             | 2 vCPU                      |
| RAM             | 4G                          |
| Stockage        | 16G                         |
| Interfaces      | eth0: VLAN115 -> 10.11.5.10 |

fty-laut01.cenexis.lan est l'instance contrôlant toutes les automatisations de cette infrastructure. Avec Ansible, cette machine déploie, installe et configure les autres instances.