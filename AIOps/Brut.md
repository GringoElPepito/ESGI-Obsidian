Contexte d'entreprise Nordel -> modèle IA interne Orion basé sur Qwen3.5.

Game Day éviter IA agentic.

Toute affirmation cite la requête, son résultat et l'horodatage.

Orion plateforme servant à faire du RAG et de la réponse au question.

Les quatres signaux d'observabilité :
- Métriques -> (Charge CPU, Temps de réponse)
- Logs -> évènements textuels 
- Traces -> Parcours distribué d'une requête à travers chaque microservice. 
- Profils -> Echantillonnage de pile. Consommation CPU et mémoire par fonction


Culture DevOps -> automatiser le maximum de choses
Culture SRI -> 
- La méthode USE sonde les ressources matérielles : 
	- utilisation : % de capacité active
	- saturation : travail en file d'attente
	- erreurs : rejets ou échecs matériels pour HTTP erreur 4XX & 5XX sauf 429.
- La méthode RED :
	- Rate -> requêtes par seconde
	- Errors -> échecs HTTP 5XX
	- Duration -> centiles p50 et P95
	- Golden Signals (Google) : RED + saturation

Prometheus - Grafana

Ollama -> PC
LLMd
vLLM -> GPU
Tensor-RT -> Nvidia


Checks :
- Nagios, Zabbix : Sondes binaire sur seuil fixe, sans corrélation

Séries :
- Prometheus : Modèle pull dimensionnel à labels et requêtes PromQL -> stack kubernetes/conteneur

Standard :
- OpenTelemetry : standard pour les métriques, traces, et logs. SDK et collecteur neutres pour traces, métriques et logs.
Plateformes :
- Datadog, Dynatrace : SaaS, tout-en-un à corrélation intégrée et coût à l'usage
- Splunk : spécialisé sécu


CheckMK -> alternative à Zabbix.

SLO 

Check de la quantité de requête sur un intervalle idéalement suffisamment long.
P95 -> 95% du trafic est égale ou inférieur

budget error est la quantité d'erreur tolérer sur l'infra.
On continue les déploiements tant que le budget n'est pas atteint.
Lorsque celui-ci est atteint on stoppe tous les déploiements et on travail sur la résilience de l'infra.

Burn rate -> vitesse à laquelle le budget error est consommé