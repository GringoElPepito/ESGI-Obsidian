```python
with open(fich.txt) as f :
	print(type(f))
	print(type.mro(type(f)))
	print(dir(f))
```
mro (Method )

mode d'ouverture :
- read : lit un fichier
- write : écraser un fichier existant ou écrire un nouveau fichier
- append : ajoute à la suite du fichier

```python
source=open("read.py","r")
print(source.read())
source.read(10) #renvoie uniquement les 10 premiers caractères
source.readline() #renvoie la première ligne complète
source.readlines() #Renvoie les lignes du fichier dans un tableau
source=open("read.py","w")
source.write('01234567899')
source=open("read.py","r+")
source.read()
source.seek(10) #permet de se déplacer du fichier en fonction du nombre de caractères
source.tell() #renvoie notre position dans le fichier
source.seek(0) #déplace le curseur au début du fichier
source.close #ferme le fichier
```
