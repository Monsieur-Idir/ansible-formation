# Ansible Formation

## Lab 1 - Poste de travail



#### Environnement

> Le serveur Ubuntu (physique) fera office de noeud Ansible Manager, sur lequel 2 serveurs virtuels sont démarrés afin d'êtres contrôlés et gérés par Ansible.



Contrôlez qu'Ansible soit installé via les commandes suivantes :

```shell
ansible --version

# Résultat
ansible 2.9.6
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.8.5 (default, Jan 27 2021, 15:41:15) [GCC 9.3.0]
```



Vérifiez la présence des serveurs virtuels :

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



Effectuez un test de connexion sur ces 2 machines :

```shell
ssh root@10.94.240.238
ssh root@10.94.240.90
```

