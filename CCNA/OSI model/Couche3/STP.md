## Spanning-tree
STP = Spanning-Tree Protocol 

Explication transmission paquet :
1. Quand un switch reçoit une trame, celui-ci renseigne l'adresse MAC source dans la table CAM en l'associant avec le port d'où vient la trame.

Création de l'arbre STP. Les SW échangent des BPDU (Bridge Proto Data Unit) pour s'accorder sur une topo de niveau exempte de boucles de commutation.
/!\ Préemption dans le STP
1. Election du Root-Bridge, basé sur le bridge-id, 
   le plus petit bridge-id est celui qui gagne, le bridge-id est composé de 2 valeurs : bridge-priority - MAC du switch
   Bridge-prio : 0 à 65535. Chez Cisco : 32768
2. Les autres switchs vont calculer leur RPC ( Root Path Cost ) pour leur RP (Root Port), le RPC correspond au cumul des coût STP des liens jusqu'au root.
   Le coût STP est basé sur la BandWidth :
	- 10Mbps = 100
	- 100Mbps = 19
	- 1Gbps = 4
	- 10Gbps = 2
	- 25Gbps = 1
Si plusieurs RP (port qui mène vers le root) : choix du RP qui a le plus petit RPC
Etats des ports :
- Listening : 15s. Ecoute d'éventuels BPDU. Aucune trame transmise
- Learning : 15s. Ecoute d'éventuels BPDU. Aucune trame transmise. Apprentissage des MAC joignables par ce port
A l'issue de ces 30s, que le port passe en :
- Forwarding : opérationnel
- Blocking : n'émet aucune trame, ni même des BPDU
RP = Root Port
DP = Designated Port port qui mène vers un hub ou terminal

Sur un lien non actif du fait du STP, un côté du câble reste en forwarding. C'est le côté du SW qui a le plus petit RPC. Si les 2 switch ont le même RPC, c'est le côté du SW qui a le plus petit bridge-id.
Le port qui reste en forwarding transmet des BPDU, ce qui permet en cas de rupture du lien principale de basculer sur ce second lien.

Un port de switch qui connait aucune MAC ne transmet aucune trame utilisateurs, Broadcast ou unicast flood.
Rôle des ports :
- RP : Root Port : Port qui mène vers le root
- AP : Alternate Port : RP en blocking
- DP : Designated Port : face à un RP ou face à un équipement qui ne fait pas de STP

1. Si même RPC en passant par plusieurs switch :
   Choix du RP qui mène vers le switch qui a le plus petit bridge-id
2. Si même RPC en passant par le même switch :
   choix du RP qui mène vers le port qui a le plus petit port-id
   Port-id est composé de 2 valeurs : port-priority - Identifiant de port
   Port-Priority est compris entre 0 et 255, par défaut chez Cisco 128

A est root
A vers B = RPC 19
A vers C = RPC 19
B vers C = RPC 4
B vers E 1 = RPC 4
B vers E 2 = RPC 4
C vers E = RPC 4
C vers D = RPC 4

A RPC = 0
B RPC = 19
C RPC = 19
D RPC = 23
E RPC = 23

port de C qui va vers B blocked
port de E qui va vers C blocked
port de E Gi3 qui va vers B blocked

Un bpdu envoyé toutes les 2 secondes

Quand un switch ayant 2 RP voit son meilleur RP tomber alors il lance débloquage du second port
le STP reste actif même sur un poerfast

802.1w ou multiple STP permet de faire un arbre STP par VLAN
Influencer le STP :
Modifier le bridge-priority :
```cisco
spanning-tree vlan N° priority VALUE
```
la valeur doit être un multiple de 4096

Modifier le coût STP d'un port :
```cisco
(config-if)#spanning-tree vlan N° cost COUT
```

Modifier le port-priority :
```cisco
(config-if)#spanning-tree vlan N° port-priority PRIORITY
```

Visualiser :
```cisco
show spanning-tree
```
