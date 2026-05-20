# Powershell
Verbes de base :
- Get - Pour obtenir quelque chose
- Set - pour définir quelque chose (modifier une valeur de propriété)
- Start - Exécuter quelque chose (un processus en général)
- Stop - pour arrêter quelque chose en cours d'exécution (un processus en général)
- Out - pour générer quelque chose
- New - pour créer quelque chose

Construction d'une instruction 

Autoriser l'exécution de script Powershell
```Powershell
Set-ExecutionPolicy unrestricted
```
un cmdlets est une commande PowerShell. Chaque commande retourne un résultat sous la forme d'un objet ou d'un tableau d'objet ou d'un objet Null

Permet de récupérer la liste des services qui sont actuellement stoppé et affiche leur noms :
```PowerShell
Get-Service | WHERE {$_.status -eq "Stopped"} | SELECT displayname
```

si on ne nomme pas une variable de boucle elle prend par défaut le nom `_` et sera appelable de la manière suivante `$_`

```Powershell
# Copie chaque élément présent dans le dossier courant 
Get-ChildItem | ForEach-Object {
	Copy-Item -Path $_.FullName -destination C:\NewDirectory\
}

# Fait la même chose que le bloc précédent mais permet de récupérer les contenus de chacun des dossiers qui seront ciblés par la boucle
Get-ChildItem | ForEach-Object {
	Copy-Item -Path $_.FullName -destination C:\NewDirectory\ -recurse
}
```