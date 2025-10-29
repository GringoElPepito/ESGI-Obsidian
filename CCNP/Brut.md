# Explication d'intro
RUN -> Administration au quotidien, gestion des incidents
BUILD -> Mis en place de l'infrastructure
ARCHITECTURE -> Design de l'infrastructure

BUILD & ARCHITECTURE fonctionne de pair


Documentation à fournir :
- DCEB -> Document Conceptuel Expression du besoin
- DTC -> Document Technique de Conception
- DTR -> Document de Tests et Recettes
- DPE/DEX -> Document de procédure d'exploitation / Document d'exploitation

Une fois tous les documents produits, ont réalisé la MEP (Mise En Production)

# CCNP ENCOR
Pour avoir le CCNP Entreprise il faut :
- CCNP ENCOR
- CCNP ENARSI

## Chapitre 1

### Network Device Communication
#### Model OSI

| Layer   | Name         | PDU              | Description                               |
| ------- | ------------ | ---------------- | ----------------------------------------- |
| Layer 7 | Application  | Data             | Interface for receiving and sending data  |
| Layer 6 | Presentation | Data             | Formatting of data and encryption         |
| Layer 5 | Session      | Transaction/data | Tracking of packets                       |
| Layer 4 | Transport    | Segment          | End-to-end communication between devices  |
| Layer 3 | Network      | Packet           | Logical addressing and routing of packets |
| Layer 2 | Data Link    | Frame            | Hardware addressing                       |
| Layer 1 | Physical     | Bit              | Media type and connector                  |

Lorsqu'une machine envoie un message, ce dernier traverse chacune des 7 couches en partant de la 7 pour aller jusqu'à la 1

Encapsulation consiste à convertir une unité de données de protocol (PDU) dans l'unité de données de protocol de la couche inférieur.


#### Model TCP/IP

| Layer   | Name        | PDU | Description                                                         |
| ------- | ----------- | --- | ------------------------------------------------------------------- |
| Layer 4 | Application |     | Réuni les couches :<br>- Application<br>- Presentation<br>- Session |
| Layer 3 | Transport   |     |                                                                     |
| Layer 2 | Reseau      |     |                                                                     |
| Layer 1 |             |     |                                                                     |

#### Forwarding and Collisions Domain

méthode CSMA/CD permet d'écouter sur le réseau et si aucun message n'est reçu alors la machine peut commencer à émettre

Type de communication :
- Unicast -> Message transmis à une seule machine
- Multicast -> Message transmis à plusieurs machines
- Broadcast -> Message transmis à tout le réseau


## Chapter 2 : Spanning tree

#### STP
Protocole du control plane servant à détecter et empêcher l'apparition de boucle sur le réseau. IEEE 802.1D, il existe plusieurs état de ports avec Spanning tree protocol

| Port States | Description |
| ----------- | ----------- |
| Disabled    |             |
| Blocking    |             |
| Listening   |             |
| Learning    |             |

Root port

<<<<<<< HEAD
Les trames BPDU (Bridge Protocol Data Unit) servent à avertir des changement de topologie au sein de l'architecture


#### RSTP

| Port |     |
| ---- | --- |
|      |     |
=======
Les trames BPDU (Bridge Protocol Data Unit) 

VLAN Trunking Protocol permet la diffusion des VLANs sur différents équipements.
Dynamic Trunking Protocol permet de négocier une liaison en trunk.
>>>>>>> 50859d62d9f6ed26e30313b2a24aa96517ca6a49

Active le mode spanning-tree portfast sur tous les ports en mode access
```config
spanning-tree portfast default
```

Active le BPDU Guard sur tous les ports en mode portfast :
```config
spanning-tree bpduguard enable default
```


Plus le niveau est élevé moins le niveau est critique
Niveau de log système :
- 7. Debug
- 6. Information
- 5. Notification
- 4. Warning 
- 3. Error
- 2. Alert
- 1. Critical
- 0. Emergency

