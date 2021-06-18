


Consignes
1 - Exécuter un playbook sur les 2 conteneurs avec le module template.
2 - Créer un fichier template/conf.j2
3 - Le playbook doit déployer ce fichier templatisé sur les 2 conteneurs avec deux valeurs différentes

app_user: toto (pour web-server)
app_user: titi (pour db-server)

Le fichier doit être déployé sur /tmp/conf.txt

4 - Vérifier que le fichier a bien été déployé sur les 2 serveurs avec la valeur différente pour les 2 conteneurs.