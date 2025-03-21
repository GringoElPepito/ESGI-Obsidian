Router On A Stick = router faisant du routage inter vlan
1 VLAN = 1 réseau IP/ domaine de broadcast
Les Trunks : lien physique transportant plusieurs VLANs -> il est nécessaire d'étiqueter/taguer les trames pour identifier leur VLAN d'appartenance. 
Norme 802.1q ou dot1q
Chez Cisco tous les VLANs connu du switch (présent dans la VLAN Database) sont autorisés par défaut sur les trunks
Pour traiter les VLANs (Lire et /ou écrire le tag), le switch doit connaître ces VLANs. donc typiquement dans un LAN tous les switchs ont la même base de données des VLANs.
Chez Cisco les VLANs dans le fichier de configuration, ils sont stockés dans flash:vlan.dat.
Par défaut, tous les ports d'un switch sont dans le VLAN 1 -> VLAN Natif (untagged).

Partage de la base de données des VLANs : VTP => Virtual Trunking Protocol
VTP est un protocole propriétaire Cisco.
Il existe 3 modes :
- Serveur : créer, modifier, supprimer, apprendre les VLANs et relayer les messages VTP. Mode par défaut
- Client : apprendre les VLANs et relayer les messages VTP.
- transparent : créer, modifier, supprimer des VLANs mais ces derniers restent locaux à ce switch et il relaye les messages VTP.
VTP fonctionne avec un numéro de révision ; à l'origine = 1
A chaque modification de VLAN, le switch envoie un message VTP et le numéro avec le numéro de révision incrémenté de 1.
Les messages VTP ne s'échangent que sur les trunks
Pour configurer VTP :
Les switchs doivent avoir le même nom de domaine VTP, le même mot de passe et on peut préciser le mode.
```cisco
en
conf t
vtp domain ESGI
vtp password toto
```
Pour réinitialiser le numéro de révision :
- passer le sw en mode transparent puis le repasser dans le mode voulu
- ou changer le nom de domaine VTP puis remettre le bon

### Trunks
Protocole proprio Cisco : DTP = Dynamic Trunking Protocol
Négocier automatique un trunk entre 2 switchs
```cisco
(config-if)#switchport encapsulation dot1q
(config-if)#switchport mode { dynamic desirable | dynamic auto | trunk }
```
Toutes les combinaisons aboutissent à un trunk sauf auto avec auto
Autorisé des VLANs sur un port trunk :
```cisco
(config-if)#switchport trunk allowed vlan { all | except | add | remove } VLANs
```
Configurer un port en mode access :
```cisco
(config-if)#switchport mode access vlan N°
```
