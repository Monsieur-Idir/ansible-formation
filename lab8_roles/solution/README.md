2 - cd roles
3 - ansible-galaxy init motd-role
4 - mv motd.j2 roles/motd-role/templates
5 - mv tasks.yaml roles/motd-role/tasks/main.yaml
6 - mv defaults.yaml roles/motd-role/defaults/main.yaml

Création du playbook :

---
- name: Call my motd-role
  hosts: all

  roles:
  - motd-role


