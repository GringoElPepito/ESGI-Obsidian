# Contexte
Infra traditionnelle avant la virtu :
- Un serveur physique par application

Inconvénients :
- Ressources des serveurs sous-utilisés
- Temps de déploiement d'une application était assez long
- Interruption de service planifié ou non, surtout pour les applications ne fonctionnant pas en mode cluster
- Beaucoup d'espace nécessaire
- Grosse consommation électrique

Infra traditionnelle avec la virtu -> consolidation :
possible d'exécuter plusieurs charges de travail sur un seul serveur

Avantages :
- Permet d'exploiter pleinement les capacités

# Virtualisation

Matérielle physique de l'hyperviseur :
- CPU (compute)
- RAM (compute)
- Disque (stockage)
- Réseau

Hyperviseur Type 1 -> OS permettant de directement créer et gérer des machines virtuelles

Hyperviseur Type 2 -> OS permettant de faire fonctionner une application qui va pouvoir créer et gérer des machines virtuelles

# ESXi

Interface de gestion :
- VMWare Host
- SSH
- vCenter
- PowerCLI

# vCenter
Point de gestion centralisé pour les ESXi, se présentant sous la forme d'une VM fonctionnant 

# SDDC
Software-Defined Data Center permet d'automatiser la création d'une infrastructure