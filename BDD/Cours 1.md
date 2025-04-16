Les bases de données ont pour but initiale la non-duplication des données.
Système de base de données :
- Mainframe
	- INGRES, a existé 12 ans sans être commercialisé, créée à l'université de Californie
	- DB2, créer par IBM, en se basant sur INGRES
	- ORACLE, l'un des systèmes les plus utilisés
- Mini Mainframe
	- Informix, Information for Unix, qui est fait pour fonctionner sur les miniMainFrame
- PC
	- SQL Server
	- MySQL/MariaDB

## Méthode Merise 
On sépare le schéma de données du schéma de traitement
### Données
1. MCD => Modèle Conceptuel de Données
2. MLD => Modèle Logique de Données + choix de type de SGBD
3. MPD => Modèle Physique de Données

Il est possible d'utiliser un UML à la place d'un MLD
Classe persistantes => classe que l'on garde
Classe
### Traitement
1. MCT => Modèle Conceptuel de Traitement
2. MOT => Modèle Opérationnel de traitement
3. Code

## Modèle Entité/Association ou MCD
La systémique (notion de biologie) concept dans lequel on définit un système qui fonctionne sur un ensemble de sous-systèmes qui sont interactions.

Un système d'informations est l'ensemble des moyens (humains, matériels) et des méthodes rapportant aux traitement des informations rencontrées dans une organisation.

Le système d'information est utile à 2 autres systèmes :
- Le système de pilotage
- Le système des opérations

Les base de données sont faîtes pour enregistrer tout l'historique.
Lors de la réalisation du modèle conceptuelle, on ne formule pas d'hypothèse technique (Interface utilisé pour l'interaction avec les données, support de stockage de la base de données etc...).

Une entité ou objet est une collection d'informations ayant un rapport entre elles

Une association ou relation est une liaison fonctionnelle entre objets :
- relation binaire, relation qui associe 2 entités
- relation ternaire, relation qui associe 3 entités
- relation n-air,

Une propriété est le plus petit élément d'information qui ait un sens en lui-même et qui sera placée soit un dans une entité, soit dans une associatio