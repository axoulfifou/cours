# Visualisation avec Grafana

Grafana permet d’avoir un aperçu visuel des données collectées par Prometheus. Grafana sera également installé sur la vm ou tourne l’app et Prometheus.

adresse: http://192.168.1.12:3000

ID: admin

MDP: pikachu8787

## Installation de Grafana

Sur la vm ***flask-app*** :

```bash
sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install grafana -y
```

Démarrer et Activer Grafana :

```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

Accéder à l’interface graphique à l’adresse :

<aside>
📎

 http://192.168.1.12:3000

</aside>

- Les identifiants par défaut sont :
    - **Nom d'utilisateur** : `admin`
    - **Mot de passe** : `admin`
- Change le mot de passe par défaut lors de la première connexion pour plus de sécurité.

## Configurer Grafana:

Sur la page d’acceuil de l’interface graphique :

cliquer sur ***add data source*** :

![image.png](image%2037.png)

Puis sur Prometheus :

![image.png](image%2038.png)

Dans la section ***connection*** renseigner l’adresse et le port de Prometheus : http://192.168.1.12:9090 (adresse [localhost](http://localhost) ne fonctionne pas , il faut l’écrire en entier) :

***Pareil pour node-exporter on laisse l’adresse de prometheus :***

![image.png](image%2039.png)

Cliquer ensuite sur ***Save & test*** en bas de la page :

![image.png](image%2040.png)

Créer un  tableau de bord :

Pour cela cliquer sur ***Dashboard***:

![image.png](image%2041.png)

Puis sur ***Add visualization***:

![image.png](image%2042.png)

Puis sélectionner Prometheus:

![image.png](image%2043.png)

Ajouter une métrique en cliquant dans le champ de recherche ***Select metric*** puis sélectionner ***flask_exporter_info :***

![image.png](image%2044.png)

Pour en rajouter une autre il faut cliquer sur le bouton ***+ Add query*** comme ci-dessus. Il faudra rajouter les métriques ***flask_http_request_duration_seconds_count*** et ***flask_http_request_created***

On peut également en rajouter via des requêtes en sélectionnant ***code*** : *(utile pour les opération comme pour la latence moyenne par exemple)*

![image.png](image%2045.png)

 Cliquer ensuite sur ***save*** :

![image.png](image%2046.png)

Renseigner un titre :

![image.png](image%2047.png)

Pour vérifier les alertes sur Grafana:

Dans le menu déroulant en haut à gauche dans ***Alerting*** → ***Alert rules*** on vois les différentes alertes, ici elle est en ***normal*** car l’application fonctionne:

![image.png](image%2048.png)

Il y a la possibilité d’utiliser des templates ce qui facilite le travail vu le nombre de requêtes possible pour recueillir les infos:

exemple template tuto xavki :

```bash
https://grafana.com/grafana/dashboards/1860
```

Pour l’importer, aller dans le menu ***Dashboard*** puis ***New***→ ***Import***

![image.png](image%2049.png)

Ici on l’importera en copier-coller avec l’ID  : On colle l’ID et on clique sur ***Load*** 

![image.png](image%2050.png)

Ensuite on renseigne la source de donnée qui est notre serveur prometheus et on clique sur ***import***:

![image.png](image%2051.png)

![image.png](image%2052.png)

On peut importer le dashboard par .json ou en copier-coller de l’ID :

![image.png](image%2053.png)

on obtient ainsi toutes les données recueillit grâce au template :

![image.png](image%2054.png)