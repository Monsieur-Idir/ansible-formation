# Ansible Formation

## Lab 2 - Déploiement



#### Configuration Ansible

> Le comportement d'une installation Ansible peut être personnalisé en modifiant les paramètres figurant dans le fichier de configuration Ansible.

Parcourez les fichiers suivants grâce aux commandes suivantes :

```shell
# Fichier de configuration général
cat /etc/ansible/ansible.cfg
```

```shell
# Fichier de configuration de l'utilisateur
cat ~/.ansible.cfg
```

```shell
# Fichier de configuration propre au Projet
cat ./ansible.cfg
```



#### Gestion des inventaires

> Un inventaire ansible constitue l’ensemble des hôtes qu’Ansible devra gérer. Ces hôtes peuvent être affectés à des groupes. L’inventaire peut également définir des variables qui s’appliques aux hôtes et aux groupes.

Consultez le fichier d'inventaire :

```shell
cat inventory

# Résultat
# Group list
# ----------
[servers:children]
web-server
db-server

# Hosts list
# ----------
[web-server]
DEV-web-server

[db-server]
DEV-db-server

[all:vars]
```

Consultez ensuite la liste des serveurs et identifiez leur adresse IP :

```shell
lxc list
# Résultat
+------------+---------+----------------------+-----------------------------------------------+-----------+-----------+
|    NAME    |  STATE  |         IPV4         |                     IPV6                      |   TYPE    | SNAPSHOTS |
+------------+---------+----------------------+-----------------------------------------------+-----------+-----------+
| db-server  | RUNNING | 10.94.240.238 (eth0) | fd42:d192:4140:c3f0:216:3eff:fe63:16c8 (eth0) | CONTAINER | 0         |
+------------+---------+----------------------+-----------------------------------------------+-----------+-----------+
| web-server | RUNNING | 10.94.240.90 (eth0)  | fd42:d192:4140:c3f0:216:3eff:fe88:a85d (eth0) | CONTAINER | 0         |
+------------+---------+----------------------+-----------------------------------------------+-----------+-----------+
```

Editez les fichiers `host_vars/DEV-web-server` et `host_vars/DEV-db-server` avec les adresses IP repérées ci-dessus.

```yaml
---
# Variables of local environment
######################################
ansible_ssh_host: <ip_address>
ansible_ssh_port: 22
ansible_ssh_user: root
```

Contrôlez maintenant qu'Ansible détecte ces hôtes via la commande suivante :

```shell
ansible all --list-hosts

# Résultat
  hosts (2):
    DEV-web-server
    DEV-db-server
```







#### Commandes Ad-Hoc

> Les commandes Ansible Ad-Hoc font appel à des modules qui peuvent être exécutés sur des hôtes ciblés.
>
> Syntaxe : `ansible <host|group> -m <module> -a '<arguments>'`



Exécutez la commande suivante :

```shell
ansible localhost -m ping
```



Effectuez maintenant un ping sur les noeuds via la commande ansible :

```shell
ansible web-server -m ping -i inventory
ansible db-server -m ping -i inventory
# Commande exécutée sur l'ensemble de l'inventaire
ansible all -m ping -i inventory
```

*Il est possible de ne pas indiquer le chemin de l'inventaire en configurant cela dans le fichier de configuration Ansible `ansible.cfg`*



Exécutez la commande suivante : `ssh root@<web-server-ip>` puis fermez la connexion (CTRL+D)

Modifiez ensuite le message de bienvenue via la commande Ansible :

```shell
ansible web-server -m copy -a 'src=files/motd dest=/etc/motd' -i inventory
```



Connectez vous à nouveau sur le serveur web-server : `ssh root@<web-server-ip>`

<!--Vous constatez que le message de bienvenue a été modifié-->

Appliquez maintenant ce même message à l'ensemble des hôtes gérés :

```shell
ansible all -m copy -a 'src=files/motd dest=/etc/motd' -i inventory
```

<!--Constatez la différence de couleur sur l'action des 2 hôtes-->

![image-20210518152912107](/home/toor/.config/Typora/typora-user-images/image-20210518152912107.png)



> Cela signifie qu'il y a eu un **changement d'état** sur l'hôte qui n'avait pas encore eu son message de bienvenue configuré.
>
> On parle alors d'**idempotence** ! Cette même commande peut être executée autant de fois que souhaitée sans modifier l'état de la machine si aucune modification n'est à apporter, et à l'inverse apportera le changement souhaité.



#### Aide Ansible

Ansible propose, pour chacun de ses modules, une aide indiquant ses arguments associés, obligatoires et facultatifs.

Exécutez et analysez le résultat de la commande : `ansible-doc copy`