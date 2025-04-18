Fonctionne comme Java, avec une VM comme la JVM de Java.
Anciennement basé sur C et du Java
Maintenant basé sur un interpréteur Python
- Créé par Guido Van Rossum, fin 1980, aux pays bas.
- Open source
- Portable tous les OS
- Plusieurs implémentations écrites en langage C.
- Autres implémentations écrites en Java et .NET

# Usage
- Prototypage rapide des app
- Calculs scientifique et imagerie
- Scripts administration système et réseaux
- Automatisation de déploiements des services
- Développement Web (Scrapy,Django)
- Gestion Base de données
- Ecosystème pour Data Scientists
- Génération et création de graphique 2D et 3D (MatPlotLib)

Exemple projet :
- Mozilla, Google Yahoo
- Nasa
- Cloud

globbsecurity.fr
afpy.org
Gérard Swinnen


# Présentation technique
- Multiparadigme
- Python s'interface bien avec C et Java
- Produit du byte-code
- Technique mixte
	- Interprétation du byte code compilé puis
	- Exécution par la machine virtuelle
- Fortement typé et dynamiquement typé
- Pour Python tout est objet et tous les types sont de classes, héritage
- Il gère lui même ses ressources, (garbage collector)
- Il intègre un système d'exception pour la gestion des erreurs
- Librairies d'un peu tout (TkInter librairie graphique)

# Implémentation
- Cpython : écrit en C
- Jython : écrit en Java
- Ironpython : écrit en C#
- PyPy : écrit en Python

# Structure
- Une instruction par ligne
- moins de 80 caractères (/ pour la continuité)
- indentation pour la définition des blocs de code

# Convention
- variables en minuscule
- constante en majuscule
- classe en UpperCamelCase
- fonction en UpperCamelCase

# Types Variables

## Booléen/bool
- true or false
- 
## String/str
- chaîne vide : s1=""
- double guillemets simple: s2=''l'oeuf''
- triple guillements : s3="""..."""
- concaténation : s1+s1
- répétition : s2*3
- indice : s2[0]
- extraction : s2[0:2]
- longueur : len(s2)
- formatage : "Hello %s" % 'World'
- itération : for x in s2 
- appartenance : 'o' in s2

# Fonctions utiles
- input() : pour récupérer une entrée utilisateur
- print() : affiche les données qui lui sont passés en paramètres
- int() : permet de convertir une variable en integer
- float() : permet de convertir une variable en float

# Opérateurs logiques
- or : OU logique
- and : ET logique
- xor : OU exclusif logique
- not : NON logique
- in : cherche une occurrence

# Conditions
### If elif else :
```python
nb=int(input("T'as dit quoi ? (Saisir un entier) : "))
if nb < 0 :
	print('feur')
elif nb > 0:
	print('chi')
else :
	print('vas y j\'ai la haine')
```

### match :
```python
match jour:
	case 1:
		return 'Lundi'
	case 2:
		return 'Mardi'
	case 3:
		return 'Mercredi'
	case 4:
		return 'Jeudi'
	case 5:
		return 'Vendredi'
	case 6:
		return 'Samedi'
	case 7:
		return 'Dimanche'
	case __:
		return 'FEUR'
```


# Boucle
### for
```python
for i in [5,7,11,13]:
	print('%d est un nombre premier' % i)
```
### while
```python
a=1

while a<7:
	print(a)
	a=a+1
print('fini')
##################################
x=0
while x>=0:
	x=x+1
	if x==5:continue
	elif x==10: break
	else: print('itération n°',x)
print('c\'est finito')
```
![[Pasted image 20241209162747.png]]
![[Pasted image 20241209184821.png]]

Un socket est une association entre une IP et un numéro de port.

```python
import socket
sock=socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# en local
sock.connect(('127.0.0.1',20337))
```
Choix du protocole de couche 3 :
- `scoket.AF_INET` pour IPv4
- `socket.AF_INET6` pour IPv6
Choix du protocole de couche 4 :
- `socket.STREAM` pour une connexion TCP
- `socket.DGRAM` pour une connexion UDP

Création d'un serveur
```python
import socket
sock=socket.socket(socket.AF_INET, socket.SOCK_STREAM)
HOST=127.0.0.1
PORT=2020
BUFFER=1024

Mysocket.bind((HOST,PORT))

while 1 :
	Myscoket.listen(2)

	connexion,adresse=Mysocket.accept()

	connexion.send(b"hello client")

	msgClient=connexion.recv(BUFFER)

	
```

Création d'un client
```python
import socket, sys

sock=socket.socket()


```