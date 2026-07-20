Héberger son propre serveur de messagerie au sein d'une infrastructure informatique industrielle permet à l'entreprise de conserver la maîtrise complète de ses communications électroniques. Cette approche garantit un meilleur contrôle des données sensibles, facilite le respect des exigences de confidentialité et de conformité, et réduit la dépendance vis-à-vis de prestataires externes. Un serveur de messagerie interne peut également être intégré aux mécanismes de sécurité existants (pare-feu, sauvegardes, authentification centralisée, supervision), offrant ainsi une meilleure résilience et une disponibilité adaptée aux contraintes des environnements industriels où la continuité des opérations est essentielle.

Voici les caractéristiques de l'instance fty-lmls01.cenexis.lan :

| Paramètres      | Valeur                      |
| --------------- | --------------------------- |
| Type d'instance | LXC                         |
| OS              | Rocky 10.2                  |
| CPU             | 4 vCPU                      |
| RAM             | 4G                          |
| Stockage        | 30G                         |
| Interfaces      | eth0: VLAN161 -> 10.16.1.20 |