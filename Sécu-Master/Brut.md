# Introduction
Le Hardening consiste à configurer 

Première méthode de hardening :
`chroot` pour change root, permet de limiter l'exécution d'un binaire à un dossier spécifique du système, c'est une restriction de chemin/dossier. Etant donné que le binaire tourne sur le système il aura accès aux composants physiques comme le kernel. Docker utilise du chroot

`cgroups` permet d'isoler les ressources, permet de limiter les ressources que peut utiliser un binaire ou un ensemble de fichier.


`systemd` est le premier processus à être lancé au démarrage du système (anciennement `initd`). il va se charger de démarrer tous les autres services du systèmes.
`systemd-nspawn`, est un type de conteneur intégré à `systemd`, combine le `chroot` et la limitation de ressources.

`grsecurity` est un Role Based Access Control/Mandatory Access Control, permet d'ajouter des politiques, des règles et des ACLs pour sécuriser le fonctionnement.

Ensemble de code applicable sur le code source du kernel Linux.
Learning mode, permet de laisser `grsecurity` apprendre les ressources utiliser par chaque processus et va donc bloquer tous les autres accès n'ayant pas été utilisé pour chaque processus durant la période d'apprentissage.
` grsecurity` est payant et no protège pas des attaques systèmes avancées

`PaX` patch kernel, rajoute des sécurités au niveau du noyau. fonctionnalité :
- ASLR++, permet de rendre aléatoire l'allocation des adresses mémoires pour chaque processus.
- Limite l'accès à la RAM soit en écriture soit en exécution.

`AppArmor` est une solution de contrôle d'accès via processus. Permet de limiter les permissions de chaque processus sur le système grâce à un ensemble de profile et de configuration spécifiques à chaque binaire. Possède aussi un learning mode. Déjà installé sur la plupart des distributions Linux. Ne protège pas les accès mémoire.

`SELinux` est un RBAC/MAC multiniveaux :
- gère les droits et accès par exécution
- gère les droits et accès par utilisateur
- Gestion des accès RAM
Est à ce jour la solution la plus puissante. La configuration de SELinux peut être assez complexe, de plus le programme a été écrit par la NSA.

`Qube OS` va lancer  une machine virtuelle pour chaque exécutable lancé grâce à l'Hyperviseur Xen. domain isolation & sandboxing. Tout est automatisé, de plus Xen est intégré et donc optimisé pour fonctionner sur `Qube OS` mais le lancement d'une VM à chaque exécution est coûteux en ressources.


BSD = Berkley Software Defintion
BSD est le prédécesseur de Linux, BSD est très proche de Linux sans pour autant être la même chose. BSD reste un différent de Linux.
`OpenBSD` est une distribution de `BSD` et est sécurisé par défaut grâce :
- Berkley Packet Filter
- Spamd -> permet de bloquer les spams par défaut
- OpenSSH ->
- LibreSSL ->
- SKey ->
- LibC ->
- W\^X -> 
- KASLR ->
- SROP ->
- httpd ->
- pledge ->
- unveil ->
Fait pour être installé sur des serveurs de production ou en firewall.

`seL4` est un système d'exploitation miniaturisé conçu pour fonctionner sur des systèmes d'électronique embarqué. C'est un système en temps réel, il respecte et garantit le temps d'exécution d'une tâche.
Contrairement à un OS classique, si on exécute un `sleep 1` `seL4` va bloquer le système pendant exactement une seconde.
`seL4` est constitué de 10000 Lignes de code.
microkernel/hypervisor
Pour l'électronique embarqué, va faire tourner des systèmes d'exploitation embarqué.

# TP 1
Création d'un chroot
```bash
mkdir -p test/bin
cp /bin/sh ./test/sh
ldd /bin/sh
mkdir test/lib
mkdir test/lib64
cp 
cp 
sudo chroot test /bin/sh
```
Permet de lister tous les fichiers dans le working directory
```
echo *
```