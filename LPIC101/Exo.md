# Commandes d'exercices

Création d'une arborescence de dossier
```bash
for i in {1..15};do mkdir -p dossier$i && cd dossier$i;done
```
Lecture d'un fichier et filtrage
```bash
cat /etc/ssh/sshd_config | grep -i "port 22"
```
Lecture d'un fichier, filtrage et enregistrement du résultat dans un fichier
```bash
cat /etc/ssh/sshd_config | grep -i "port 22" > ssh_port
```
Comptage du nombre de ligne dans un fichier
```bash
cat /etc/passwd | wc -l #WC = Word Count
```
Déplacement dans un dossier avec un chemin absolu
```bash
cd /home/kaisen/dossier1/dossier10
```
Création de deux fichier
```bash
touch fichier1 fichier2
```
Triage d'un fichier et vérification de l'unicité
```bash
sort /etc/passwd | uniq -u
#ou
sort -u /etc/passwd
#ou
cat /etc/passwd | uniq -u | sort
#ou
cat /etc/passwd | sort -u
```
Ecriture d'information à la suite du contenu d'un fichier déjà existant
```bash
echo "Lisez le contenu du fichier /etc/passwd. Assurez-vous qu'il n'existe aucun doublon, après ça, classez par ordre alphabétique les éléments du fichier." >> /home/kaisen/ssh_port
```
Modification des droits d'un fichier, RW pour propriétaire et aucun droits pour les autres, tout en supprimant les droits spéciaux
```bash
sudo chmod 0600 ssh_port
```
Modification du propriétaire et du groupe propriétaire d'un fichier
```bash
chown root:root ssh_port
#ou
chown root: ssh_port #Prend le groupe primaire de l'utilisateur
#ou
chown :root ssh_port #Change le groupe propriétaire du fichier
```
Comptage du nombre de caractère
```bash
wc -m /etc/shadow
```
Ajouter 5 lignes à un fichier sans supprimer son contenu
```bash
for ((i=1; i<=5; i++));do echo "ici" | sudo tee -a ssh_port; done
#ou 
for i in {1..5};do echo "ici" | sudo tee -a ssh_port; done
```
Formater un disque
```bash
sudo fdisk /dev/sdb
n #Pour create partition
[Enter]
[Enter]
[Enter]
[Enter]
w
```
Installer un système de fichier sur une partition
```bash
sudo mkfs.ext4 -b 4096 /dev/sdb1
# ou
sudo mkfs -t ext4 /dev/sdb1
```
Monter une partition
```bash
mkdir /mnt/data
mount /dev/sdb1 /mnt/data
```
Création d'un fichier
```bash
cd /mnt/data
touch test
```
Changement propriétaire 
```bash
sudo chown kaisen:kaisen test
```
Création utilisateur
```bash
sudo useradd -s /bin/zsh utilisateur
```
Création d'un groupe
```bash
sudo groupadd esgi
```
Création d'un utilisateur complet
```bash
sudo useradd -s /bin/zsh -m -c "Utilisateur invité" -g esgi -G sudo invite
```
Création de plusieurs dossiers en même temps
```bash
sudo mkdir -p /opt/share/{paris,lyon}
#ou
sudo mkdir -p /opt/share/paris /opt/share/lyon
```
Création des groupe
```bash
sudo groupadd -g 10000 paris
sudo groupadd -g 10001 lyon
sudo groupadd -g 10999 employés
```
Changement des propriétaires et droits
```bash
sudo chown root:employés /opt/share
sudo chown root:paris /opt/share/paris
sudo chown root:lyon /opt/share/lyon
sudo chmod 750 /opt/share/lyon /opt/share/paris
```
# Commandes supplémentaires

Compte le nombre de byte d'un fichier
```bash
wc -c #Compte le nombre de Byte
```
Compte le nombre de caractère
```bash
wc -m #Compte le nombre de caractère
```
Supprimez tous les doublons
```bash
uniq -u
```
Naviguez au dossier personnel
```bash
cd ~
```
Affiche toutes les lignes ne commençant ni par # ni par $ 
```bash
grep -v -E ^"(#|$)" /etc/apt/sources.list
# -v permet d'inverser la condition
# -E permet d'utiliser les extendeds regex
```
Lister les disque connecter 
```bash
sudo fdisk -l
```
Création utilisateur
```bash
adduser # script PERL
useradd # programme compilé
```
Vérification de l'existence d'un groupe ou utilisateur
```bash
getent passwd utilisateur
getent group utilisateur
```
Ajout d'un mot de passe sur un groupe
```bash
sudo gpasswd kaisen
```
Récupérer ids utilisateur
```bash
id kaisen
```
Valider installation certificat serveur nginx
```bash
ssl_certificate
```