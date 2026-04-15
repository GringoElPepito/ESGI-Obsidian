# Création de l'autorité de certification
## ToDo Manual
- [x] Update packages
- [x] Install needed packages `sudo dnf in -y openssl`


## ToDo Manual with easy-rsa
```bash
sudo dnf up -y
sudo dnf in -y epel-release
sudo dnf in -y easy-rsa
mkdir ~/easy-rsa
ln -s /usr/share/easy-rsa ~/easy-rsa
chmod 700 ~/easy-rsa
cd ~/easy-rsa/3.2.4
./easyrsa init-pki
```