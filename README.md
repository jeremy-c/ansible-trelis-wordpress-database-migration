# Ansible Role to provision remote Wordpress database #

## How to use ##
Add the following lines to your group_vars/production or group_var/staging files. 
Replace [site-name] with the name of your site.


```
wordpress_sites:
  [site-name]:
    development:
      name: linksmith.nl.dev
      wp_home: http://www.linksmith.nl.dev
      db_name: d-linksmith

```

Create Ansible playbook file "database.yml" in project root and add the following lines: 

```
---
- name: Migrate database between environments
  hosts: web
  remote_user: "{{ web_user }}"

  vars:
    project: "{{ wordpress_sites[site] }}"
    project_root: "{{ www_root }}/{{ site }}"

  roles:
    - database-migration
```

### Example ###

 ```
ansible-playbook -i hosts/production database.yml --extra-vars="site=[site-name]"
ansible-playbook -i hosts/staging database.yml --extra-vars="site=[site-name]"
 ```
 
## What does this role do? ##
 
* dump local DB
* rsync dump to remote host
* dump remote DB
* import local dump to remote DB
* search-replace references to local domain with remote domain