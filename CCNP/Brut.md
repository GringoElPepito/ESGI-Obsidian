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
Model OSI

| Layer   | Name         | PDU         | Description                               |
| ------- | ------------ | ----------- | ----------------------------------------- |
| Layer 7 | Application  | Data        | Interface for receiving and sending data  |
| Layer 6 | Presentation | Data        | Formatting of data and encryption         |
| Layer 5 | Session      | Transaction | Tracking of packets                       |
| Layer 4 | Transport    | Segment     | End-to-end communication between devices  |
| Layer 3 | Network      | Packet      | Logical addressing and routing of packets |
| Layer 2 | Data Link    | Frame       | Hardware addressing                       |
| Layer 1 | Physical     | Bit         | Media type and connector                  |

Lorsqu'une machine envoie un message, ce dernier traverse chacune des 7 couches