Lors de l'envoie de données, les données vont descendre le modèle OSI (de la couche x(7 à 2) vers la couche 1). A chaque couche traversé, les données vont être encapsulée ceci ajoutant des informations nécessaires à la transmissions des données.
Lors de la réception de données, les données vont remonter le modèle OSI (de la couche 1 à la couche x(couche depuis laquelle l'envoie des données à été initié)). A chaque couche traversée, l'en-tête correspondante à la couche est retiré laissant donc l'en-tête suivante lisible.
Schéma : 
A
P [data]
S
T [N°| port src | port dest [data]] = [Segment TCP]
N [IP src | IP dest [Segment TCP]] = [Packet IP]
DL [MAC src | MAC dest [Packet IP]] = [Trame Ethernet]
P [Trame Ethernet] -> 01010101010
Phrase mnémotechnique : All People Seem To Need Data Processing
