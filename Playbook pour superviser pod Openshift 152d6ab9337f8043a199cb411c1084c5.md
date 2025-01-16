# Playbook pour superviser pod Openshift

Le but ici est de superviser le contenu d’un bucket dans un pod OpenShift qui est déclenché automatiquement via un cronjob.

Nous avons donc un fichier **inventory.ini** sert à spécifier les hôtes gérer avec Ansible, ici c’est simplement [localhost](http://localhost) car c’est le pod qui l’executera.

Le fichier **`/host_vars/localhost/connection.yml`**configure des variables spécifiques pour l'hôte `localhost`. Ixi `ansible_connection: local` indique qu'Ansible n'utilisera pas SSH pour se connecter à cet hôte. Au lieu de cela, il exécutera les commandes localement.

Le fichier **alerting.yml** qui permet de définir les différentes tâches à exécuter , c’est ce qu’on apelle un **playbook**

Il faut monter le playbook dans un configmap :

exemple :

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ansible-playbooks-config
  namespace: wdx-sdev
data:
  alerting.yml: |
    - hosts: localhost
      gather_facts: false
      ... *reste du playbook*....

```

Puis dans le cronjob :