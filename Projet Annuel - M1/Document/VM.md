Nous avons autant que possible favorisé l'utilisation de LXC dans le but de réduire la quantité de ressources consommés par nos différents services. Cependant dans certains cas, l'utilisation d'une VM s'avère être une meilleur option. 
Pour pouvoir garantir un niveau de sécurité répondant à notre contexte pour chacune de nos VM, nous avons créé une template avec une configuration de durcissement basé sur les recommandations de l'ANSSI.
Dans un premier temps, nous avons réalisé un partitionnement sur le disque système. Voici le découpage de nos partitions :
- `/` : Partition racine accueillant tout le système
- `/boot` : Partition contenant les éléments nécessaires au démarrage du système
- `/boot/efi` : Partition con