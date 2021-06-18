# Ansible Formation

## Lab 9 - Ansible Galaxy



1 - Télécharger le rôle Tomcat de Zaxos à partir d'Ansible Galaxy

`ansible-galaxy install zaxos.tomcat-ansible-role`

Pensez à préciser avec l'argument -p le répertoire où déposer le rôle.

> La bonne pratique voudrait que l'installation du rôle se fasse via le fichier roles/requirements.yaml



2 - Créer le playbook appelant le rôle Tomcat et exécutez le sur le conteneur de votre choix.

