Le déploiement des LXC est entièrement automatisé à travers le playbook deploy_lxc.yml. Voici comment ce dernier se découpe :

| Rôle                         | Action                                                                   |
| ---------------------------- | ------------------------------------------------------------------------ |
| lxc/check_if_exists          | Vérifie que le LXC n'existe pas déjà, si c'est le cas stoppe le playbook |
| lxc/select_ct_template       |                                                                          |
| lxc/create_ct_from_template  |                                                                          |
| lxc/configure_ct             |                                                                          |
| proxmox/add_pool_member      |                                                                          |
| proxmox/set_ha_resources     |                                                                          |
| common/start_instance        |                                                                          |
| common/reset_known_host      |                                                                          |
| common/get_facts             |                                                                          |
| common/update_packages       |                                                                          |
| common/install_packages      |                                                                          |
| common/set_certificate_facts |                                                                          |
| common/get_certificates      |                                                                          |
| common/request_certificate   |                                                                          |
| common/request_certificate   |                                                                          |
| syslog/pki_files             |                                                                          |
| syslog/push_conf             |                                                                          |
| syslog/logrotate_conf        |                                                                          |
| siem/agent                   |                                                                          |
| common/add_services_fwcmd    |                                                                          |
| common/setsebool             |                                                                          |
