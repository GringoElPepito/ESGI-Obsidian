# Histoire de Linux


**Alias**
alias c=clear permet de taper `c` pour exécuter la commande `clear`

FHS = File Hierarchy Standard
tmpfs=Temporary Filesystem est système de fichier temporaire 

Interdiction de mettre 777 en droit sur un répertoire ou un fichier.

Commande pour récup le nombre de cœur d'un processeur.
![[Pasted image 20241021151041.png]]
# Répertoire Linux




Pas mettre de mot de passe a root pour créer un utilisateur avec sudo

# Fichier utilisateur/groupe

## LS -lhi
```bash
784925 -rw-r--r-- 1 kaisen:kaisen 4.0K Nov 19 10:18 fichier
```
- 784925 : inode
- -rw-r--r-- : droit du fichier
- 1 : nombre d'inode
- kaisen:kaisen : propriétaire:groupe
- 4.0K : nombre de Byte
- Nov 19 10:18 : Date de dernière modification
- fichier : nom du fichier
`ls -A` : affiche tous les fichiers mais n'affiche pas `.`  & `..`
`ls -a` : affiche tous les fichiers

# Device
sda = SCSI Device a


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