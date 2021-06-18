# Ansible Formation

## Lab 8 - Les rôles



#### Création du rôle

1 - Au sein même de votre repository, créez la structure de votre rôle avec le nom 'motd'.

<!--exemple : lab8/roles/<name_role>-->

*ansible-galaxy init `<name_role>` peut aider* 

2 - Déplacez et renommez le fichier playbook.yaml dans le dossier correspondant aux tâches.

3 - Déplacez le fichier motd.j2 dans le dossier correspondant aux templates.

4 - Déplacez le fichier defaults.yaml dans le dossier correspondant aux templates.



#### Utilisation du rôle

1 - Créer votre playbook permettant d'appeler le rôle que vous venez de créer, sur l'hôte que vous choisirez.

2 - Modifiez la valeur de la variable `system_owner` dans votre playbook par 'votre_prénom'.

*Inutile d'y ajouter des tâches.*

