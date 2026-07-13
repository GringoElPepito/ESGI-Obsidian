Bien trop souvent délaissé dans les plus petites infrastructures, le serveur Syslog est un outil parfaitement adapté pour retracer le déroulement d’une attaque informatique ou encore d’un incident rendant hors d’usage certaines machines. 

Ce service a pour simple but de réceptionner les logs, à savoir les informations des événements, de l’ensemble des machines de l’infrastructure. Cela permet ainsi de centraliser l’intégralité des logs à un seul et unique endroit, facilitant ainsi leur accès et renforçant la disponibilité des logs.

Pour la centralisation des logs, nous avons optés pour la solution Rsyslog, une solution open source représentant la dernière itération de l’outil syslog et présentant de nombreux avantages en comparaison à ces prédécesseurs.

L’outil Syslog a connu plusieurs itérations, la première nommé Syslog, supportait uniquement l’ UDP et ne pouvait donc pas garantir le transport jusqu’à la destination. De plus, il ne supportait pas le chiffrement de données, ce qui représente un problème de sécurité assez important.

La seconde itération nommé Syslog-ng supporte le transport par TCP et le chiffrement TLS, remédiant ainsi aux lacunes de son prédécesseur.

La dernière itération à ce jour de cet outil est nommé rsyslog, il possède les mêmes fonctionnalités que syslog-ng tout en ajoutant le support du protocole RELP. Là où TCP ne permet pas de renvoyés les logs si la connexion a été interrompue entre le client et le serveur, RELP se charge de renvoyés les logs qui n’ont pas pu l’être.

Voici les caractéristiques de l'instance syslog.cenexis.lan :
- CPU : 1 core
- RAM : 512 Mb
- Stockage : 32G
- Instance : LXC
- OS : Rocky Linux 10.2
- état HA attendu : started
- Réseau
	- VLAN : VLAN 161 - Syslog & SIEM
	- IP : 10.16.1.10
{screen - Summary syslog.cenexis.lan}

Rsyslog est un service pouvant être configuré en tant que serveur ou client.
En tant que serveur, celui-ci réceptionne les logs transmis par les machines clientes et les enregistrent en respectant les règles définis dans le ou les fichiers de configuration. En tant que client, celui-ci transmet automatiquement chaque message d’évènement au moment où il est émis par le système, évitant ainsi que les logs soient modifiés avant leur envoie.

Dans notre cas, nous configurons le service pour qu'il fonctionne en mode serveur. Notre Rsyslog étant installé sur un Rocky Linux, cela implique qu’il faut ouvrir le port sur lequel le service va écouter pour recevoir les logs envoyer par les clients. Dans notre cas le port est 20514, le port par défaut est le 514, nous avons volontairement choisi un port différent pour des raisons de sécurité. Pour poursuivre dans ce sens l'intégralité des échanges réalisé entre les clients rsyslog et le serveur sont chiffrés à l'aide d'un algorithme de chiffrement Post-Quantique, à savoir ML-DSA65 supporté par TLSv1.3. Ainsi nous pouvons garantir un niveau de sécurité maximal concernant la transmission des logs de nos différentes instances limitant ainsi les risques que ceux-ci soit interceptés et décrypter.
Pour faciliter la navigation entre les logs, nous avons fait en sorte que chaque hôte remontant ses logs sur le serveur ait son propre dossier et qu'un fichier de log soit créer par process. De cette manière, il est bien plus facile de trouver les logs que l'on souhaite.
{screen - Configuration rsyslog }