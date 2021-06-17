# JpetStore - Lab Ansible



Procédure :
https://topic.alibabacloud.com/a/installation-jpetstore-detailed-steps_8_8_20195558.html


#### Préparation de l'environnement de travail
`lxc launch ubuntu:16.04 db-server`
`lxc launch ubuntu:16.04 web-server`



1 - Créer un nouveau ansible (mkdir lab-jpetstore)
2 - Créer la structure
    * /files/fichiers.sql
        * /host_vars/
        * inventory
        * playbook-mysql.yaml (qui lui s'exécute donc sur db-server)
        * playbook-tomcat.yaml (qui lui s'exécute donc sur web-server)

3 - Rédiger le playbook MYSQL qui fera les actions suivantes :
    - Créer un groupe 'jpetgroup' (avec un GID 7001) - module: group
    - Créer un utilisateur 'jpetuser' (avec un UID 7001) - module: user
    - Créer un répertoire '/dists' qui contiendra les packages (avec les droits jpetgroup/jpetgroup - droits 755) - module: file
    - Télécharger MySQL (download d'un tar.gaz, ou apt install)
    - Installer MySQL

4 - Rédiger le playbook Tomcat qui fera les actions suivantes :
    - Créer un groupe 'jpetgroup' (avec un GID 7001) - module: group [utilisez des variables dans host_vars]
    - Créer un utilisateur 'jpetuser' (avec un UID 7001) - module: user [utilisez des variables dans host_vars]
    - Créer un répertoire '/dists' qui contiendra les packages (avec les droits jpetgroup/jpetgroup - droits 755) - module: file
    - Télécharger JEE
    - Télécharger Tomcat
    - Installer Tomcat

    - Télécharger les binaires et les mettre dans le répertoire /dists (le fichier .war)
    - Installer MySQL - module apt


Binaires :
- MySQL Java Connecter : https://downloads.mysql.com/archives/get/p/3/file/mysql-connector-java-3.1.4-beta.tar.gz
- MySQL 4.1.12 : http://mysql.he.net/Downloads/MySQL-4.1/mysql-4.1.12.tar.gz


Composants et versions :
- MYSQL:4.1.12-NT
- TOMCAT:5.5.9
- JDK:1.5.0_03





    - 



https://sourceforge.net/projects/ibatisjpetstore/files/jpetstore4/4.0.5/iBATIS_JPetStore-4.0.5.zip/download?use_mirror=pilotfiber&download=