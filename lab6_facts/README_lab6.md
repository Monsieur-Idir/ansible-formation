# Ansible Formation

## Lab 6 - Gestion des "facts"



##### Préparation de l'environnement

```shell
lxc launch ubuntu:16.04 test-server
lxc file push --uid 0 --gid 0 ~/.ssh/id_rsa.pub test-server/root/.ssh/authorized_keys
lxc list
```

Editez le fichier `host_vars/DEV-test-server` et renseignez l'adresse IP correspondante.



#### Les "facts"

> Les faits Ansible sont des variables détectées automatiquement par Ansible sur un hôte géré.
> Les faits contiennent des informations propres à l'hôte qui peuvent être utilisées comme des
> variables régulières dans les plays, les conditions, les boucles ou toute autre instruction liée à une
> valeur collectée à partir d'un hôte géré. 

Souvenez vous que, par défaut, Ansible récupère les "facts" de l'hôte ou groupe cible avant chaque exécution d'un playbook.

Consultez le playbook playbook.yaml et exécutez le : `ansible-playbook playbook.yaml -i inventory`

Parcourez le résultat et toutes les informations récupérées.



Il peut être nécessaire de récupérer une valeur parmis ces informations, comme par exemple l'adresse IP.

De ce fait, exécutez le playbook_2.yaml : `ansible-playbook playbook_2.yaml -i inventory`



##### Désactivation du "gathering_facts"

Il est possible de désactiver la fonctionnalité de Gathering Facts pour plusieurs raisons éventuelles :

- Souhait d’accélérer l’exécution du playbook.
- Réduire la charge générée par le play sur les hôtes gérés.
- Exécution du setup non pris en charge par l’hôte (raison quelconque).
- Installation pré-requis de logiciel avant de rassembler les facts.

Pour la désactivation :

```yaml
---
- name: "LAB6 - Gestion des 'facts'"
  hosts: test-server
  become: True
  gather_facts: no
```

Exécutez ensuite le playbook `playbook_3.yaml` et constatez le résultat : `ansible-playbook playbook_3.yaml -i inventory`

