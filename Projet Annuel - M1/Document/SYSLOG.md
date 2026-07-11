Bien trop souvent délaissé dans les plus petites infrastructures, le serveur Syslog est un outil parfaitement adapté pour retracer le déroulement d’une attaque informatique ou encore d’un incident rendant hors d’usage certaines machines. 

Ce service a pour simple but de réceptionner les logs, à savoir les informations des événements, de l’ensemble des machines de l’infrastructure. Cela permet ainsi de centraliser l’intégralité des logs à un seul et unique endroit, facilitant ainsi leur accès et renforçant la disponibilité des logs.

Pour la centralisation des logs, nous avons optés pour la solution Rsyslog, une solution open source représentant la dernière itération de l’outil syslog et présentant de nombreux avantages en comparaison à ces prédécesseurs.

L’outil Syslog a connu plusieurs itérations, la première nommé Syslog, supportait uniquement l’ UDP et ne pouvait donc pas garantir le transport jusqu’à la destination. De plus, il ne supportait pas le chiffrement de données, ce qui représente un problème de sécurité assez important.

La seconde itération nommé Syslog-ng supporte le transport par TCP et le chiffrement TLS, remédiant ainsi aux lacunes de son prédécesseur.

La dernière itération à ce jour de cet outil est nommé rsyslog, il possède les mêmes fonctionnalités que syslog-ng tout en ajoutant le support du protocole RELP. Là où TCP ne permet pas de renvoyés les logs si la connexion a été interrompue entre le client et le serveur, RELP se charge de renvoyés les logs qui n’ont pas pu l’être.

Voici les caractéristiques de l'instance syslog.cenexis.lan :
- CPU : 1 core
- RAM : 512 Mb
- Stockage : 16G
- Instance : LXC
- OS : Rocky Linux 10.2
- état HA attendu : stopped
- Réseau
	- VLAN : VLAN 112 - Active Directory & CA
	- IP : 10.11.2.20
{screen - Summary root-ca.cenexis.lan}

Rsyslog est un service pouvant être configuré en tant que serveur ou client.
En tant que serveur, celui-ci réceptionne les logs transmis par les machines clientes et les enregistrent en respectant les règles définis dans le ou les fichiers de configuration. En tant que client, celui-ci transmet automatiquement chaque message d’évènement au moment où il est émis par le système, évitant ainsi que les logs soient modifiés avant leur envoie.