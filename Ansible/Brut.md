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
	
}
```