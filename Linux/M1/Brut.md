# Mesure de l'utilisation des ressources
Il existe 2 commandes pour surveiller la consommation de ressources de notre machine :
- `top` -> 
- `htop`  (pour human `top`) -> 

`htop` permet de tuer un service en cours de fonctionnement.

# Analyse des ports 
Il existe 2 commandes pour analyser les ports connecté ou en écoute :
- `netstat` 
- `ss` -> nouvelle version de `netstat`

elles affichent plusieurs informations comme
- Le protocole utilisé `tcp` ou `udp` généralement
- Le nombre de paquets reçus
- Le nombre de paquets envoyés
- L'adresse IP et le port local
- L'adresse IP et le port distant si en communication, sinon les adressages et ports acceptés
- L'état du port
	- `LISTEN` : Le port est en écoute
	- `TIME_WAIT` : Après la fermeture d'une session, le port se met dans cette état pour reprendre la connexion rapidement en cas de coupure involontaire
	- `ETABLISHED` : connexion établie
- Le processus

# Voir les processus
On peut utiliser `top` ou `htop`, cependant `ps` permet d'avoir une visualisation beaucoup plus poussés de vos processus.

TTY = console local
PTS = Terminal virtuelle

Options de `ps` :
- `-a` permet d'afficher les processus des autres utilisateurs
- `-u` permet d'afficher les processus liés à l'utilisateurs lançant la commande
- `-x` permet d'obtenir des informations détaillés

La commande `kill` permet de tuer un processus, en rajoutant l'option `-9` permet de tuer tous les processus de la même famille que le processus spécifié

# Regarder le temps d'activité de la machine

`uptime` permet de voir le temps écoulé depuis le dernier démarrage de la machine

# Regarder les utilisateurs connectés

`who` permet de lister tous les utilisateurs actuellement connectés

`wall` permet d'envoyer un message aux autres utilisateurs connectés

`hostnamectl` permet d'afficher plusieurs info sur la machine comme par exemple si la machine est une VM.

# Réseau

L'interface réseau de loopback permet la communication entre les services 
Sous Ubuntu le service permettant de gérer le réseau est `netplan`
Un fichier de configuration `netplan` est généré lors de l'installation de la VM Ubuntu
`/etc/netplan/99-netcfg-vmware.yaml` :
```YAML
network:
	version: 2
	renderer: networkd
	ethernets:
		ens192:
			dhcp4: no
			dhcp6: no
			addresses:
				- 10.66.131.97/25
			routes:
				- to: default
				  via: 10.66.131.1
				- to: 192.168.252.0/24
				  via: 10.66.131.1
				- to: 192.168.254.0./24
				  via: 10.66.131.1
			nameservers:
				addresses: [127.0.0.1, 10.16.16.1, 10.16.17.1]
		ens224:
			dhcp4: no
			dhcp6: no
			addresses:
				- 10.66.133.97/25
		 
		
```

Sur debian 

Les fichiers utiles :
- `/etc/hosts` : associe des adresses IP à des noms d'hôtes
- `/etc/hostname` : hostname de l'OS
- `/etc/networks` : associe des noms d'hôtes à une adresses IP
- `/etc/services` : nom des services tcp/udp ainsi que leur numéro de port, sert principalement pour que les automates puissent configurer automatiquement le pare-feu pour faire fonctionner le service.
- `/etc/resolv.conf` : configuration DNS (priorisation Netplan sur Ubuntu)

`arp`

`watch` permet de lancer en boucle une commande,
La commande suivante va lancer un ping de 4 paquets vers l'IP 1.1.1.1 toutes les 2 secondes
```bash
watch ping 1.1.1.1 -c 4
```

La commande `ping` permet de réaliser du Port Knocking,
Le Port Knocking est une méthode permettant d'activer dynamiquement un service en utilisant une séquence de ping précise.

`traceroute` permet de voir la route empruntée pour atteindre une destination réseau.

`mtr` est une version améliorer de `traceroute` permettant de tester en continu la route empruntée pour atteindre une destination réseau.

`tcptraceroute` permet de donné la route empruntée comme si on avait fait un `curl`

Il est possible d'enregistrer statiquement certaine route même si cette route utilise la passerelle par défaut, de cette manière si l'on veut changer la route par défaut pour en tester une nouvelle on va pouvoir maintenir les routes pour certaines destination

## Gestion de route
Ajouter une route  non-persistante dynamiquement :
```bash
ip route add IP/CIDR gw GATEWAY
```

Supprimer une route persistante dynamiquement :
```bash
ip route del IP/CIDR gw GATEWAY
```

## Scan de port
`nmap` permet de scanner des ports

## Capture de paquets
`tcpdump` permet de capturer les paquets entrants.

## Debug DNS
Solutions possibles :
- `host` -> `host IP SERVEURDNS`
- `dig` -> 
- `nslookup` -> 

# Gestion des paquets

Les deux principaux packages managers :
- APT (Advanced Package Tool) : Utilisé par les distributions basées sur Debian, telles qu'Ubuntu
- YUM/DNF (Yellowdog Updater Modified/Dandified YUM) : Utilisé par les distributions telles que Red Hat/Cent OS/Alma/Rocky et Fedora

`apt` fourni une sortie qui n'est pas réellement exploitable via un script.
Pour les scripts on privilégiera `apt-get`
## Configuration des dépôts
Pour simplifier l'installation et la mise à jour des logiciels, Linux utilise des dépôts, également appelés référentiels ou repositories. Les dépôts sont des bibliothèques en ligne qui contiennent des packages de logiciels prêts à être installés sur votre système.

### Ubuntu/Debian
Pour Ubuntu et Debian, on configure les dépôts avec le fichier `/etc/apt/sources.list` ainsi que le dossier `/etc/apt/sources.list.d/` 
```/etc/apt/sources.list
deb https://
```

- jammy: C'est le nom de code de la version spécifique d'Ubuntu pour laquelle ce dépôt est configuré, dans ce cas "jammy" pourrait être le nom de code ''


### RedHat
On configure les dépôts de DNF et YUM avec le dossier `/etc/yum.repos.d/`

Voici un exemple de configuration :
```INI
[rhel-9-for-x86_64-appstream-rpms]
name = Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)
baseurl = https://cdn.redhat.com/content/dist/rhel9/$releasever/x86_64/appstream/os
enabled = 1
gpgcheck = 1
gpgkey = file:///etc/pki/

```

Recherche de package fonctionne avec les regex, la commande suivante va retourner tous les packages commençant par `ansible` :
```bash
dnf search ^ansible
```

## Gestion des droits fichiers

Les permissions :
- Lecture (r) : Permet de lire le contenu du fichier ou du répertoire
- Ecriture (w) : Permet de modifier le fichier ou d'ajouter, supprimer ou renommer des fichiers dans un répertoire
- Execution (x) : Pour un fichier, permet de l'exécuter en tant que programme ou script. Pour un répertoire permet d'accéder à son contenu

Le Sticky Bit permet de limiter la suppression du fichier au créateur du fichier et à root.

Chaque permission est représentée par un chiffre :
- 4 -> Lecture (read)
- 2 -> Ecriture (write)
- 1 -> Execution (execute)

### Attributs
`lsattr` permet de lister les attributs d'un fichier
Liste des attributs :
- `a` append only -> Permet uniquement l'ajout de contenu dans le fichier en bloquant tout le reste (Suppression, modification des métadonnées, modification des droits etc...)
- `i` immuable -> Empêche toute modification du fichier
- `s` 
- `e` ext4 -> précise 

### Sudo
On spécifie les utilisateurs ayant le droit d'utiliser sudo dans le fichier `/etc/sudoers` on modifiera ce fichier uniquement avec `visudo`

Voici le format :
```sudo
user hôte=(utilisateur_cible) commande
```

Exemple :
```sudo
toto ALL=(ALL) ALL
```




## Log
Les types de logs :
- `Syslog` : Logs système de journalisation principal dans Ubuntu
- `Auth Logs` : Ces journaux enregistrent les informations d'authentification, y compris les tentatives de connexions réussies ou échouées
- `Kernel Logs` : Ces logs contiennent des informations spécifiques au noyau du système
- `Dmesg Logs` : Ces logs contiennent les messages du noyau système au moment du démarrage. Ces messages sont générés par le noyau Linux pendant le processus de démarrage du système. La commande `dmesg` existe également qui permet d'afficher les logs du démarrage (sur RedHat, il n'a pas de fichier de lof `dmesg`, il faudra obligatoirement passer par la commande).
- `Apache/Nginx Logs` : Si vous avez un serveur web Apache ou Nginx, les journaux de ces serveurs enregistrent les requêtes clientes.

On trouve les logs dans le fichier logs dans le dossier `/var/log`

`tail -f /var/log/nom_du_fichier.log` : Cette commande affiche la fin du fichier tout en affichant les informations ajoutées à celui-ci en temps réel

`sudo grep -Ri toto /var/log/` : 

Rotation des logs : les logs sont des fichiers pouvant être très volumineux, il faut donc optimisé leur gestion

On peut configurer la rotation des logs dans le fichier `/etc/logrotate.conf` ou dans le dossier `/etc/logrotate.d/`. Les conf s'appliquent dynamiquement
```bash
weekly #Toutes les semaines le dimanche à minuit
```
Permet de spécifier le nombre d'itération de log avant suppression
```
rotate 4
```
## Brut
Linux possède 3 Kernel :
- Version en cours d'utilisation
- Version précédente
- Version qui a été installé à la création de la machine (fallback)