# Ansible Formation

## Lab 3 - Playbooks



##### Préparation de l'environnement

```shell
lxc launch ubuntu:16.04 test-server
lxc file push --uid 0 --gid 0 ~/.ssh/id_rsa.pub test-server/root/.ssh/authorized_keys
lxc list
```

Editez le fichier `host_vars/DEV-test-server` et renseignez l'adresse IP correspondante.



#### Ecriture et exécution de playbooks

> Les commandes ad-hoc peuvent exécuter une tâche simple et unique sur un ensemble d’hôtes ciblés.
>
> Mais la puissance réelle d’Ansible réside dans l’utilisation de playbooks pour exécuter plusieurs tâches complexes sur un ensemble d’hôtes ciblés d’une manière facilement reproductible.



But de l'exemple : Faire la différence entre une suite de tâches exécutées par les commandes ad-hoc et un playbook Ansible.

<u>Commandes Ad-Hoc</u>

- Création d'utilisateur : `ansible test-server -m user -a 'name=elyette group=root state=present' -i inventory`

- Installation du package NGINX : `ansible test-server -m apt -a 'name=nginx state=present' -i inventory`

- Configuration de NGINX : `ansible test-server -m copy -a 'src=files/nginx.conf dest=/etc/nginx/sites-enabled' -i inventory`

- Redémarrage du serveur NGINX : `ansible test-server -m service -a 'name=nginx state=restarted' -i inventory`



<u>Playbook</u>

Consultez le playbook `playbook.yaml` et exécutez les commandes suivantes :

```shell
# Contrôle de la syntaxe du playbook
ansible-playbook playbook.yaml --syntax-check

# Exécution à blanc (simulation) du playbook
ansible-playbook -C playbook.yaml -i inventory

# Exécution du playbook
ansible-playbook playbook.yaml -i inventory

# Exécution du playbook avec niveau de log
ansible-playbook playbook.yaml -i inventory -vvv
```



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