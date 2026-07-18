## Mise en place LDAPS

Création d'un fichier `request.inf` :
```inf
[Version]
Signature="$Windows NT$"

[NewRequest]
Subject = "CN=fty-wads01.cenexis.lan, OU=Community, O=CENEXIS, L=Fontenay-sous-Bois, S=Ile-de-France, C=FR"
MachineKeySet = TRUE
KeyLength = 4096
KeySpec = 1
KeyUsage = 0xA0
Exportable = TRUE
SMIME = FALSE
RequestType = PKCS10
HashAlgorithm = SHA256
ProviderName = "Microsoft Software Key Storage Provider"
ProviderType = 0

[Extensions]
2.5.29.17 = "{text}"
_continue_ = "dns=fty-wads01.cenexis.lan"
```

Création d'une demande de signature :
```powershell
certreq -new request.inf fty-wads01.csr
```

Copie du fichier de demande de signature sur l'autorité de certification intermédiaire :
```powershell
scp ./fty-wads01.csr desigual@10.11.2.21:/home/desigual/req/
```

Sur l'autorité de certification intermédiaire :
```bash
cd /home/desigual/sub-ca
sudo openssl ca -batch -config sub-ca.conf -subj "/CN=fty-wads01.cenexis.lan/OU=Community/O=CENEXIS/L=Fontenay-sous-Bois/S=Ile-de-France/C=FR" -create_serial -in req/fty-wads01.csr -out certs/fty-wads01.crt -extensions server_ext -days 365 -passin pass:<pswd_private_key_sub_ca>
sudo openssl crl2pkcs7 -nocrl -certfile certs/fty-wads01.crt -certfile sub-ca.crt -certfile root-ca.crt -out certs/fty-wads01.p7b
sudo cp certs/fty-wads01.p7b ../
sudo chown desigual:desigual ../fty-wads01.p7b
```

Récupération du certificat sur l'AD :
```powershell
scp desigual@10.11.2.21:/home/desigual/req/fty-wads01.p7b ./
```

Installation du certificat :
```powershell
certreq -accept .\fty-wads01.p7b
```

Vérification du LDAPS :
```powerhsell
netstat -ano | findstr :636
```
