# Infrastructure pour l'IA
2% de perte de paquet = x8 en temps d'entrainement IA

Une requête simple requiert une grande quantité de ressources

Chatbot (pic de consommation) =! Agentic (consommation constante)

## Dimensionnement des systèmes IA
Private data center 're-acceleration' mis en place d'infra IA On-premise

400Gb/s par GPU pour une infra IA

L'IA est donc directment impacté par l'infrastructure réseau sur laquelle elle repose
Pour ce genre d'infrastructure, TCP/IP est mis de côté

Agentic Design Pattern

Hyper-distributed security and observability découpage de l'application en micro-services 

Utilisation d'ASIC sont des puces dédiés à la réalisation de tâches spécifiques
NPU : Network Processor Unit

Infra :
as-a-service
Traditionnal APplications | Agentic AI Applications
Application Platforms
Compute and Data
Network, Silicon and Optics

Security => Everything as Code
Observability => Sustainability
- Vérifier l'état du système
- Vérifier les performances

Mis en place d'une infra par 
- Virtualisation (VMware/Nutanix)
- Conteneurisation (Kubernetes/OpenShift)

1. Prompt -> 
2. Tokenize Embed (Transformation du langage naturelle en nombre)-> 
3. Prefill (plongement du nombre dans un monde vectorielle à X dimensions, calcul simple réalisable avec des GPUs optimisés pour réaliser des grandes quantités de calculs simple) -> 
4. KV Cache | Model Weights -> 
5. Decoding Loop (Autoregressive) -> 
6. De-Tokenize



D