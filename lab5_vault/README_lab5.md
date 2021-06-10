# Ansible Formation

## Lab 5 - Ansible Vault, les secrets



##### Préparation de l'environnement

```shell
lxc launch ubuntu:16.04 test-server
lxc file push --uid 0 --gid 0 ~/.ssh/id_rsa.pub test-server/root/.ssh/authorized_keys
lxc list
```

Editez le fichier `host_vars/DEV-test-server` et renseignez l'adresse IP correspondante.



#### Les secrets

> Il est possible qu'Ansible ait besoin d'accéder à des données sensibles telles que des mots de passe ou clés API pour configurer les hôtes gérés. 

Consultez le playbook `playbook.yaml` et contstatez l'appel du fichier de variable `vars/vault.yaml`.

```yaml
---
- name: "LAB5 - Ansible Vault and secrets"
  hosts: test-server
  become: True
  vars_files:
    - vars/vault.yaml

  tasks:
  - name: Create secret user and password
    user:
      name: "{{secret_user}}"
      password: "{{secret_user_pwd}}"
```



Exécutez ensuite le playbook : `ansible-playbook playbook.yaml -i inventory`

L'erreur suivante devrait apparaître : `ERROR! Attempting to decrypt but no vault secrets found`

*Ceci s'explique par le fait que le fichier de variables contenant des informations sensibles a été encrypté avec `ansible-vault`.*

Décryptez le fichier avec la commande suivante : `ansible-vault decrypt vars/vault.yaml` (mot de passe : `vault`)

L'exécution du playbook est alors possible, mais sachez qu'il n'est pas nécessaire de décrypter le fichier avant l'exécution, ce n'est d'ailleurs pas une bonne pratique.



Ré-encrypter le fichier vars/vault.yaml avec la commande : `ansible-vault encrypt vars/vault.yaml`.
*Pensez à conserver le mot de passe, ou conserve le mot de passe précedemment utilisé : `vault`.*

Exécutez une nouvelle fois le playbook cette fois-ci avec l'argument vault : `ansible-playbook playbook.yaml -i inventory --ask-vault-pass`
