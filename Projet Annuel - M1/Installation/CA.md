# Création de l'autorité de certification
## ToDo Manual
- [x] Update packages
- [x] Install needed packages `sudo dnf in -y openssl`

# Script openssl

```bash
mkdir root-ca
cd root-ca
vi root-ca.conf
mkdir certs db private
chmod 700 private
touch db/index
openssl rand -hex 16  > db/serial
echo 1001 > db/crlnumber
openssl req -new \
	-config root-ca.conf \
    -out root-ca.csr \
    -keyout private/root-ca.key
openssl ca -gencrl \
	-config root-ca.conf \
	-out root-ca.crl
openssl ca -selfsign \
    -config root-ca.conf \
    -in root-ca.csr \
    -out root-ca.crt \
    -extensions ca_ext
openssl req -new \
    -newkey rsa:2048 \
    -subj "/C=FR/O=LUDOVIK/CN=OCSP Root Responder" \
    -keyout private/root-ocsp.key \
    -out root-ocsp.csr
openssl ca \
    -config root-ca.conf \
    -in root-ocsp.csr \
    -out root-ocsp.crt \
    -extensions ocsp_ext \
    -days 30
openssl ocsp \
    -port 9080
    -index db/index \
    -rsigner root-ocsp.crt \
    -rkey private/root-ocsp.key \
    -CA root-ca.crt \
    -text
openssl ocsp \
    -issuer root-ca.crt \
    -CAfile root-ca.crt \
    -cert root-ocsp.crt \
    -url http://127.0.0.1:9080
```


## ToDo Manual with easy-rsa
```bash
sudo dnf up -y
sudo dnf in -y epel-release
sudo dnf in -y easy-rsa
mkdir ~/easy-rsa
ln -s /usr/share/easy-rsa/* ~/easy-rsa/
chmod 700 ~/easy-rsa
cd ~/easy-rsa/3.2.4
./easyrsa init-pki
echo 'set_var EASYRSA_REQ_COUNTRY "FR"
set_var EASYRSA_REQ_PROVINCE "Ile-de-France"
set_var EASYRSA_REQ_CITY "Fontenay-sous-Bois"
set_var EASYRSA_REQ_ORG "CENEXIS"
set_var EASYRSA_REQ_EMAIL "admin@cenexis.lan"
set_var EASYRSA_REQ_OU "Community"
set_var EASYRSA_ALGO "rsa"
set_var EASYRSA_DIGEST "sha512"
' > pki/vars
sudo cp pki/ca.crt /etc/pki/ca-trust/source/anchors/
sudo update-ca-trust
```

# Revoke certificate
```bash
openssl ca \
	-config root-ca.conf \
    -revoke certs/1002.pem \
    -crl_reason keyCompromise
```
