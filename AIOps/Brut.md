Contexte d'entreprise Nordel -> modèle IA interne Orion basé sur Gwen3.5.

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

