`ansible.cfg` est le fichier de configuration d'Ansible, il contient différent paramètre comme l'emplacement du fichier d'inventaire, l'emplacement des logs.

il est possible de créer des fichiers playbook on l'exécute à l'aide de la commande
```bash
ansible-playbook 
```

Fichier d'inventaire :
```INI
[ESGI]
localhost ansible_connection=local
```

Syntaxe d'une tâche Ansible :
```bash
---
- name: Affichage OS
  debug:
	  var: ansible_os_family
```

une variable créée par un register contient toujours les champs suivant :
```json
{
	"changed": bool
	"rc": int
	"failed": bool
}
```

Pour créer une boucle on utilise `loop` :
```YAML
- name: Affichage
  debug:
	var: item
  loop: "{{ class_list }}"
  loop_control:
	label: "{{ item.name }}"
```

Il existe l'instruction `block`, cette instruction possède 2 avantages, elle permet de gérer les cas d'erreur avec les instructions `rescue` et `finally`. Le second avantage est qu'il permet de généraliser des paramètres d'exécution  (`when`, `become` etc..):
```YAML
---
- name: Create a block
  block:
	  - name: TASK1
	    debug:
		  msg: "Pas de problème"
  rescue:
	  - name:
	    debug:
		  msg: "Il y a une erreur"

```

l'instruction `notify` permet de déclencher un handler très utile pour redémarrer des services suite à la modification de leur configuration.

Arborescence Role Ansible :
- `defaults`
- `files`
- `handlers`
- `meta`
- `tasks`
- `templates`
- `tests`
- `vars`

Il existe certain