# Ansible Formation

## Lab 4 - Variables et "facts"



##### Préparation de l'environnement

```shell
lxc launch ubuntu:16.04 test-server
lxc file push --uid 0 --gid 0 ~/.ssh/id_rsa.pub test-server/root/.ssh/authorized_keys
lxc list
```

Editez le fichier `host_vars/DEV-test-server` et renseignez l'adresse IP correspondante.



#### Inclusions de variables

> Ansible prend en charge des variables qui permettent de stocker des valeurs en vue de leur 	réutilisation dans l’ensemble des fichiers au sein d’un projet Ansible.
>
> Les variables peuvent être définies à des emplacements divers au sein d'un projet Ansible.



##### Etendue globale

Exécutée à partir d'une commande ad-hoc, les variables définies ici sont les plus prioritaires :

`ansible test-server -m command -a 'echo {{message}}' -i inventory -e 'message=hello'`



##### Etendue de play

Consultez le playbook `playbook.yaml` et visualisez les variables `user`, `home` et `package` : 

```yaml
---
- name: "LAB4 - Variables and facts"
  hosts: web-server
  become: True
  vars:
    user: Albert
    home: /home/albert
    package: nginx
```

Il est également possible d'appeler un ou plusieurs fichiers de variables :

```yaml
---
- name: "LAB4 - Variables and facts"
  hosts: web-server
  become: True
  vars_files:
    - vars/users.yaml
    - vars/packages.yaml
```

Les variables sont alors définies dans ces fichiers :

```yaml
user: Albert
home: /home/albert
```



L'appel de ces variables dans le playbook se fait via des accolades {{}} :

```yaml
  - name: Create the user {{Albert}}
    user:
      name: "{{user}}"
      home: "{{home}}"
```



**Important** : Lorsqu'une variable est utilisée comme premier élément commençant une valeur,
l'utilisation de guillemets est impérative



##### Etendue d'hôtes

Les variables d'hôtes peuvent êtres définies au niveau des hôtes au travers :

- de l'inventaire

  ```yaml
  [test-server:vars]
  server_name=test-server
  
  [all:vars]
  env="DEV"
  sys_proxy_host="localhost"
  sys_proxy_host="3128"
  ```

  > Cette méthode n'est pas conseillée car elle compléxifie la gestion de l'inventaire et pourrait apporter des erreurs par le mélange d'informations au sein d'un même fichier.

- des `group_vars`

  ```yaml
  ---
  logging_level: 'INFO'
  app_system_user: admin
  app_system_user_id: 7001
  app_system_group: admingrp
  app_system_group_id: 7001
  ```

- des `host_vars`

  ```yaml
  ---
  # Variables of local environment
  ######################################
  ansible_ssh_host: 10.94.240.223
  ansible_ssh_port: 22
  ansible_ssh_user: root
  ```



La méthode à donc privilégier est celle des fichiers host_vars et group_vars.
Prenez le temps d'observer l'inventaire `inventory2` et la relation des noms de `host` et de `group`.

L'inventaire spécifie :

- Le groupe `servers`, rattaché au fichier `group_vars/servers`;
- les hosts `DEV-web-server`, `DEV-db-server` et `DEV-test-server`;
- Le groupe `all` qui partage les variables pour l'ensembles des hôtes gérés.

Il est donc important de faire correspondre les noms présents dans l'inventaire avec les noms de fichiers des `group_vars` et `host_vars`.









##### Exercice

> Conseils pour cet exercice :
>
> - Utilisez l'aide avec la commande `ansible-doc <module>`
> - Soyez vigilent à l'indentation



Ecrivez votre propre playbook qui effectuera les étapes suivantes :

- Création de l'utilisateur <votre_nom> avec l'UID et le GID 1501
- Installation et démarrage du service HTTPD
- Copie du fichier de configuration apache vers le serveur
- Redémarrage du serveur apache