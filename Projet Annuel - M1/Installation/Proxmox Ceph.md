Création de partition de disque 
```bash
sgdisk -n 1:0:1T /dev/sda
```
- `-n` permet définir les paramètres de création de la partition
- `1` -> numéro de partition
- `0` -> commence la partition au premier block disponible
- `1T` -> Taille de la partition
- `/dev/sda` -> chemin vers le disque cible

