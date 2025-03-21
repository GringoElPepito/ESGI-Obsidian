## Agrégation de liens
Entre 2 switchs -> agréger 2,4,8 ou 16 liens pour augmenter la BW
2 protocoles :
- PagP : Port Aggreg Proto : proprio Cisco
- LACP : Link Aggre Control Protocol 802.3ad
Algo de distribution entre les 2 switchs pour utiliser les x liens, il y a 2 familles d'algo :
- Binaire : basé sur un seul paramètre (IP-source, IP-destination, Mac-source, Mac-destination, Port-source, Port-destination).
- Hashage : XOR entre 2 paramètres (IP-source XOR IP-destination, Mac-source XOR Mac-destination, Port-source XOR Port-destination).
A noter : les 2 switchs ne doivent pas forcément utiliser le même algo
- Les ports d'un bundle doivent avoir la même vitesse.
- Si c'est un trunk, il faut forcer en mode trunk (pas de dynamic possible)
- Avoir les même VLANs autorisés
Configurer un port channel :
Choix de l'algo
```cisco
port-channel load-balance [algo]
```
Bundlisation des ports :
```cisco
int range e1/2-3
channel-protocol { pagp | lacp(lacp) }
channel-group 1 mode { desirable(pour pagp) | auto(pour pagp) | active(lacp) | passive(lacp) }
```
Visualition du port-channel :
```cisco
sh etherchannel summ
```
Visualisation load-balancing :
```cisco
sh ethernetchannel load-balance
```
