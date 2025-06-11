# Gestionnaire de paquet APT
APT est une surcouche d'abstraction utilisant DPKG en arrière-plan. La différence est que DPKG ne fait pas de résolution de dépendances. C'est APT qui s'en occupe 
Met à jour la liste des paquets téléchargeables mais ne mets pas à jour les paquets
```bash
sudo apt update
```
Met à jour les paquets sans régler les soucis de dépendances
```bash
sudo apt upgrade
```
Met à jour les paquets en réglant les soucis de dépendances
```bash
sudo apt full-upgrade
```
Réparer les problèmes de dépendances
```bash
sudo apt install -f
```
Lister les paquets disponible sur les repos
```bash
sudo apt list
```
Lister les paquets installés avec DPKG
```bash
dpkg -l
```
Supprimer un paquet
```bash
sudo apt remove
```
Supprimer un paquet et ses dépendances si elles ne sont pas utilisés
```bash
sudo apt autoremove
```
Supprimer un paquet, ses dépendances non utilisés et les fichiers de configuration
```bash
sudo apt autoremove --purge
```
Supprimer les configurations d'un paquet désinstallé
```bash
sudo dpkg --purge nom_du_paquet
```