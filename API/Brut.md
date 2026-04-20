# Qu'est-ce qu'une API ?
Un site web classique renvoie une page HTML qui est interprété par le client (Navigateur Web)

La révolution de l'API, renvoie des données au format JSON 

Application Programming Interface

Le backend (API web) permet de centraliser la logique pour fournir plusieurs clients.

Facilite l'ajout de fonctionnalité au sein d'une application (ex: moyen de paiement).

swagger -> permet de lister l'ensemble des actions (endpoint) atteignables via l'API.

L'API permet de cacher une partie de la logique de l'application dans le but de la garder privé et d'exposer uniquement les fonctionnalités souhaitées
On peut limiter l'utilisation de la totalité ou une partie des fonctionnalités aux utilisateurs authentifiés.


# C'est quoi une API REST ?

Les différents types d'API :
- SOAP - Simple Object Access Protocol
- REST - REpresentational State Transfer
- GraphQL
- RPC

- Une API REST doit être stateless, le serveur n'a pas besoin de se souvenir des requêtes précédentes.
- Possibilité de mise en cache des données
- Interface uniforme
- Code HTTP
- Utilisation des méthodes HTTP
	- GET
	- POST

Versioning de l'API :
- /api/v1
- /api/v2

Pagination, Filtrage et Tri

# Format d'une requête 
```
GET /api/posts/12 HTTP/1.1
Host: jsonplaceholder.typicode.com
Accept: application/json
Authorization: Bearer fuzçecioezhnciuzencviozebcuizec
User-Agent: Mozilla/5.0
```

```
HTTP/1.1 200 OK Date: 
Server: nginx/1.18.0
Content-Type: application/json
Content-Length: 245
Cache-Control: 
```

# Découpage d'une URL HTTP

`https://jsonplaceholder.typicode.com/posts/1/comments?_limit=5&_sort=id`
- `https://` = Protocol
- `jsonplaceholder.typicode.com` = Domaine
- `/posts` = Ressource
- `/1` = Path Parameter
- `/comment` = Sous-resource
- `?` = séparateur URL parameters
- `_limit=5&_sort=id` = paramètres


