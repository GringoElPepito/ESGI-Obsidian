RAG :
- Retrieval -> Récupération, aller chercher dans un corpus, les passages proches de la question.
- Augmented -> Augmentée, Ajouter ces passages au prompt, à côté de la question.
- Generation -> Génération, Le modèle répond à partir de ce qu'il vient de lire

# Pourquoi donner à lire au modèle ?
Corpus VALDORNE mutuelle -> 71 documents internes d'une mutuelle fictive

Token portion d'un mot, en Français on a en moeynne 1,6 token par mot.

Les Transformers permettent de lire la phrase d'un coup plutôt que mot à mot. Les transformers utilisent plus de données, plus de puissance de calcul pour aboutir à des modèles plus performants.

Les LLM ne mentent pas, il reconstruit. S'il ne parvient pas trouver le fait chercher il renvoie ce qui y ressemble

Pourquoi ne pas coller ? 

Mémoire de travail -> Ce que l'IA est capable de voir d'un seul coup (équivalent à la RAM). Les LLM sont stateless, et ne conserve donc aucune mémoire entre chaque appel. Les échanges de la fenêtre de contexte n'est pas stocké par le LLM, ils sont reconstruit à chaque appel, l'application autour du modèle renvoie l'historique des échanges pour simuler une conversation, ce qui a un impact sur le coût
Mémoire des poids -> Ce que l'IA connaît 

Plus de tokens signifie plus de coût, des réponses plus lentes et parfois un modèle qui perd de vue ce qui a été dit plutôt dans la conversation. On appelle cela le pourrissement du contexte

Zone efficace de contexte entre 30 et 50% au-delà une perte de performance peut être constatée.

Le contexte se remplit à chaque question et à chaque réponse.

La longueur des documents est ce qui a le plus d'impact dans le remplissage du contexte.

Fine-Tuning consiste à réentrainer un modèle dans le but de modifier ses poids

Les poids d'un modèle sont 

# Comment choisit-on ce qu'il lit ?

# où cela casse-t-il et que répare-t-on ?

# Mesurer et savoir ne pas faire de RAG
