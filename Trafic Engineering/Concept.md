Voici la liste détaillée des termes et concepts présents dans le document, organisés par thématiques avec leur **définition/fonctionnalité** et leur **rôle dans le réseau**.

---

### 1. Concepts généraux et métriques réseau

* **QoS (Quality of Service / Qualité de Service)**
  * **Définition & Fonctionnalité :** Mécanisme permettant de donner la priorité à certains trafics critiques (voix, vidéo) au détriment de trafics moins sensibles (téléchargements) lorsque le réseau est saturé.
  * **Rôle :** Garantir des performances adaptées aux besoins de chaque application sur un réseau IP convergent où tous les flux partagent le même câble.
* **Traffic Engineering (TE)**
  * **Définition & Fonctionnalité :** Envisagé ici sous l'angle de la QoS (bien qu'au sens strict lié à MPLS et BGP).
  * **Rôle :** Optimiser l'acheminement du trafic et la gestion des ressources réseau.
* **Bande passante (BP / Bandwidth)**
  * **Définition & Fonctionnalité :** Débit maximal ou largeur de transmission d'un lien réseau. Le débit disponible est limité par le lien le plus lent du parcours.
  * **Rôle :** Déterminer la quantité maximale de données transmises par unité de temps.
* **Délais (Latence)**
  * **Processing delay :** Temps nécessaire au routeur pour analyser l'en-tête de la trame et déterminer où l'orienter.
  * **Queueing delay :** Temps passé par le paquet à attendre dans la file d'attente d'une interface.
  * **Serialization delay :** Temps nécessaire pour placer tous les bits d'un paquet sur le support physique (dépend de la taille du paquet et du débit).
  * **Propagation delay :** Temps de trajet des bits d'un bout à l'autre du câble (dépend de la distance).
  * **Rôle global :** Mesurer le temps total mis par un paquet pour traverser le réseau.
* **Gigue (Jitter)**
  * **Définition & Fonctionnalité :** Variation du délai d'acheminement entre plusieurs paquets d'une même conversation.
  * **Rôle :** Indique la régularité d'un flux ; une forte gigue rend la voix saccadée ou robotique.
* **Perte de paquets & Tail Drop**
  * **Définition & Fonctionnalité :** Suppression de paquets lorsqu'une file d'attente est pleine (le *Tail Drop* jette systématiquement les derniers paquets arrivés).
  * **Rôle :** Éviter l'engorgement indéfini de la mémoire des routeurs, mais peut provoquer une dégradation forte de la qualité applicative.

---

### 2. Téléphonie sur IP et multimédia

* **Codec (ex. G.711, G.729)**
  * **Définition & Fonctionnalité :** Algorithme qui numérise et compresse la voix analogique à l'émission, puis fait l'inverse à la réception. (G.711 = 64 kbps non compressé ; G.729 = 8 kbps compressé).
  * **Rôle :** Adapter le flux vocal au débit disponible sur le réseau.
* **RTP / UDP / IP (En-têtes voix)**
  * **Définition & Fonctionnalité :** Ensemble des en-têtes (40 octets au total : IP 20 octets, UDP 8 octets, RTP 12 octets) ajoutés aux paquets de voix.
  * **Rôle :** Acheminer les paquets multimédias en temps réel avec séquencement et horodatage.

---

### 3. Modèles d'architecture QoS

* **Best-Effort**
  * **Définition & Fonctionnalité :** Modèle par défaut d'Internet sans aucune garantie ni priorité (premier arrivé, premier servi).
  * **Rôle :** Traiter tous les paquets de façon équitable sans traitement particulier.
* **IntServ (Integrated Services)**
  * **Définition & Fonctionnalité :** Modèle basé sur la **réservation explicite et préalable** de ressources de bout en bout avant la transmission du flux.
  * **Rôle :** Garantir un niveau de service strict à chaque flux, mais manque de passage à l'échelle sur de très grands réseaux.
* **RSVP (Resource ReSerVation Protocol)**
  * **Définition & Fonctionnalité :** Protocole de signalisation utilisé par IntServ (messages PATH, RESV, Teardown).
  * **Rôle :** Réserver la bande passante nécessaire sur chaque routeur le long du chemin.
* **CAC (Call Admission Control)**
  * **Définition & Fonctionnalité :** Contrôle d'admission d'appels.
  * **Rôle :** Refuser un nouvel appel vocal si la bande passante disponible est insuffisante pour ne pas dégrader les appels existants.
* **DiffServ (Differentiated Services)**
  * **Définition & Fonctionnalité :** Modèle basé sur le marquage individuel des paquets en bordure de réseau via une étiquette.
  * **Rôle :** Permettre aux routeurs de cœur de traiter les paquets par classes de trafic sans conserver l'état de chaque flux (très évolutif).

---

### 4. Marquage des paquets (Couches 2 et 3)

* **ToS (Type of Service)**
  * **Définition & Fonctionnalité :** Champ d'un octet (8 bits) situé dans l'en-tête IP.
  * **Rôle :** Transporter les informations de priorité au niveau 3.
* **IP Precedence**
  * **Définition & Fonctionnalité :** Codage historique sur les 3 premiers bits du champ ToS (valeurs de 0 à 7).
  * **Rôle :** Indiquer le niveau d'importance du paquet IP (ex. 5 pour la voix).
* **DSCP (Differentiated Services Code Point)**
  * **Définition & Fonctionnalité :** Codage moderne sur les 6 premiers bits du champ ToS (64 valeurs possibles).
  * **Rôle :** Offrir une classification beaucoup plus fine des flux dans le modèle DiffServ.
* **PHB (Per-Hop Behavior)**
  * **Définition & Fonctionnalité :** Comportement d'acheminement appliqué par un routeur à un paquet en fonction de son DSCP.
  * **EF (Expedited Forwarding) :** Traitement à faible latence, faible gigue et faible perte (DSCP 46 / EF = 101110) dédié à la voix.
  * **AF (Assured Forwarding) :** Traitement garantissant une bande passante minimale avec gestion des niveaux de rejet (AFxy : x = classe, y = probabilité de suppression).
* **CoS (Class of Service / 802.1p)**
  * **Définition & Fonctionnalité :** Champ de 3 bits présent dans les trames Ethernet 802.1Q/ISL au niveau 2 (valeurs de 0 à 7).
  * **Rôle :** Prioriser le trafic au sein d'un réseau local (LAN/Switchs).
* **Bits DE (Frame Relay) / CLP (ATM)**
  * **Définition & Fonctionnalité :** Indicateurs de perte au niveau 2 (*Discard Eligibility* / *Cell Loss Priority*).
  * **Rôle :** Désigner les trames ou cellules à jeter en priorité en cas de congestion sur les liaisons WAN spécialisées.

---

### 5. Outils de configuration Cisco

* **MQC (Modular QoS CLI)**
  * **Définition & Fonctionnalité :** Architecture modulaire Cisco d'administration de la QoS en 3 parties distinctes :
    * **Class-map :** Définit *qui* fait partie de la classe (identification du trafic).
    * **Policy-map :** Définit *quoi faire* pour chaque classe (attribution des garanties/priorités).
    * **Service-policy :** Applique la politique sur une interface réseau (en entrée ou sortie).
* **NBAR (Network Based Application Recognition)**
  * **Définition & Fonctionnalité :** Moteur d'inspection profonde des paquets (couches 4 à 7).
  * **Rôle :** Identifier dynamiquement les applications complexes, à ports variables ou masquées (HTTP URL, Citrix, Peer-to-Peer).
* **PDLM (Packet Description Language Module)**
  * **Définition & Fonctionnalité :** Fichiers d'extension pour NBAR.
  * **Rôle :** Ajouter la reconnaissance de nouveaux protocoles applicatifs sans réinstaller le système.
* **AutoQoS**
  * **Définition & Fonctionnalité :** Fonction de configuration automatisée de la QoS par une commande simple.
  * **Rôle :** Déployer automatiquement les meilleures pratiques et configurations QoS sur les routeurs et switchs.

---

### 6. Algorithmes de gestion de la congestion (Queueing / Files d'attente)

* **FIFO (First In First Out)**
  * **Définition & Fonctionnalité :** Une seule file d'attente sans distinction.
  * **Rôle :** Traitement chronologique simple.
* **PQ (Priority Queueing)**
  * **Définition & Fonctionnalité :** Traitement strict par niveaux de priorité (High, Medium, Normal, Low).
  * **Rôle :** Vider entièrement les files prioritaires avant de traiter les autres (risque d'affamer les files basses).
* **WRR (Weighted Round Robin) & DRR (Deficit Round Robin)**
  * **Définition & Fonctionnalité :** Tour de rôle pondéré entre plusieurs files. DRR mémorise les octets envoyés en trop au tour précédent pour corriger l'équité.
  * **Rôle :** Distribuer la bande passante de manière proportionnelle entre les files.
* **WFQ (Weighted Fair Queueing)**
  * **Définition & Fonctionnalité :** Découpage automatique des paquets par flux/conversations via un hachage.
  * **Rôle :** Accorder du temps aux petits flux interactifs et empêcher les gros flux de monopoliser la liaison.
* **CBWFQ (Class-Based WFQ)**
  * **Définition & Fonctionnalité :** Combinaison de la MQC et de WFQ pour attribuer une bande passante garantie par classe.
  * **Rôle :** Assurer un débit minimal réservé à chaque type de données applicatives.
* **LLQ (Low Latency Queueing)**
  * **Définition & Fonctionnalité :** CBWFQ augmenté d'une file prioritaire stricte avec limiteur de débit.
  * **Rôle :** Traiter la voix en priorité absolue sans affamer le reste du trafic en cas de surconsommation.

---

### 7. Évitement de la congestion

* **RED (Random Early Detection)**
  * **Définition & Fonctionnalité :** Suppression aléatoire de paquets avant que la file ne soit complètement saturée.
  * **Rôle :** Inciter les sessions TCP à réduire leur fenêtre d'émission pour éviter l'effet de *Tail Drop* massif.
* **WRED (Weighted RED)**
  * **Définition & Fonctionnalité :** RED couplé aux marquages IP Precedence ou DSCP.
  * **Rôle :** Commencer à éliminer les paquets de faible priorité plus tôt que les paquets prioritaires.
* **ECN (Explicit Congestion Notification)**
  * **Définition & Fonctionnalité :** Marquage d'un bit dans l'en-tête IP plutôt que la suppression du paquet.
  * **Rôle :** Avertir l'émetteur de la présence d'un bouchon afin qu'il ralentisse sans provoquer de perte de données.

---

### 8. Régulation de débit (Traffic Conditioning)

* **Policing**
  * **Définition & Fonctionnalité :** Limitation stricte du débit avec suppression ou ré-étiquetage immédiat des paquets excédentaires.
  * **Rôle :** Appliquer une limite nette sans ajouter de latence.
* **Shaping**
  * **Définition & Fonctionnalité :** Lissage du débit en stockant les paquets excédentaires dans une file pour les transmettre plus tard.
  * **Rôle :** Réduire les pertes au prix d'un délai supplémentaire.
* **Métriques de contrat (CIR, Bc, Be, Tc)**
  * **CIR (Committed Information Rate) :** Débit moyen garanti par l'opérateur.
  * **Bc (Committed burst) :** Volume maximal de données autorisées par intervalle.
  * **Be (Excess burst) :** Volume additionnel toléré au-delà du Bc.
  * **Tc :** Intervalle de temps de calcul (\\(\text{Tc} = \text{Bc} / \text{CIR}\\)).

---

### 9. Optimisation et Sécurité

* **Compression d'en-têtes (cRTP)**
  * **Définition & Fonctionnalité :** Réduction de la taille des en-têtes IP/UDP/RTP de 40 octets à 2-4 octets.
  * **Rôle :** Libérer de la bande passante sur les liens WAN lents.
* **Fragmentation & Interleaving (LFI / Link Fragmentation and Interleaving)**
  * **Définition & Fonctionnalité :** Découpage des gros paquets de données et insertion (*interleaving*) des paquets voix entre les morceaux.
  * **Rôle :** Éviter que les paquets vocaux sensibles n'attendent la fin de la sérialisation d'un gros paquet de données sur une liaison lente.
* **Préclassification QoS (`qos pre-classify`)**
  * **Définition & Fonctionnalité :** Copie ou mémorisation des critères de classification avant le chiffrement d'un paquet dans un tunnel VPN (IPsec/GRE).
  * **Rôle :** Permettre l'application des règles QoS en sortie même si le contenu réel du paquet est chiffré et masqué.

---

💡 **Souhaites-tu un schéma récapitulatif ou une fiche de révision ciblée sur l'une de ces parties particulières (comme les calculs de délai ou les mécanismes MQC/LLQ) ?**