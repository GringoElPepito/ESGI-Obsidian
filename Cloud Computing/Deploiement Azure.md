Pour le déploiement à partir d'un groupe de ressources, il faut supprimer les lignes de dépendances circulaires ainsi que l'id des managedDisk (Azure gère lui même le nommage des disques) et osProfile.requireGuestProvisionSignal (Option indisponible en mode student).
Il faut aussi ajouter le paramètres `adminPassword` :
```template.json
    "parameters": {
        "adminUsername": {
			"defaultValue": "ferreira",
            "type": "string"
        },
        "adminPassword": {
            "type": "secureString"
        },        
        "emailRecipient": {
            "defaultValue": "mferreira55@myges.fr",
            "type": "string"
        },
```
Une fois les paramètres renseignés on peut les appeler de la manière suivante :
```
    "osProfile": {
         "computerName": "[parameters('virtualMachines_VMHUB_name')]",
         "adminUsername": "[parameters('adminUsername')]",
         "adminPassword": "[parameters('adminPassword')]",
```

Supprimer les dépendances circulaires 
Ajouter dépendances sur les réseaux virtuels
