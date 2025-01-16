# Surveillance et monitoring : Prometheus

adresse : http://192.168.1.12:9090

Permet de récolter les informations concernant mon application et de configurer des alertes (par exemple, si l’application tombe). Il sera installé sur la même vm que sur laquelle l’application  ***flask-app*** tourne

## Installation de Prometheus :

Sur la machine où tourne l’application (flask-app-12 | 192.168.1.12) télécharger la dernière version de Prometheus :

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.47.0/prometheus-2.47.0.linux-amd64.tar.gz
```

Extraire l’archive téléchargée :

```bash
tar xvf prometheus-2.47.0.linux-amd64.tar.gz
```

Créer un dossier /etc/prometheus et déplacer les fichiers extraits dedans :

```bash
sudo mkdir /etc/prometheus
sudo cp prometheus-2.47.0.linux-amd64/prometheus /etc/prometheus/
sudo cp prometheus-2.47.0.linux-amd64/promtool /etc/prometheus/
sudo cp -r prometheus-2.47.0.linux-amd64/consoles /etc/prometheus/
sudo cp -r prometheus-2.47.0.linux-amd64/console_libraries /etc/prometheus/
sudo cp prometheus-2.47.0.linux-amd64/prometheus.yml /etc/prometheus/
```

Créer un utilisateur Prometheus :

```bash
sudo useradd --no-create-home --shell /bin/false prometheus 
# créer un utilisateur sans créer son répertorie /home car inutile et spécifie le shell /bin/false qui empêche l'utilisateur de se connecter au système.
```

Ajout des permissions pour l’utilisateur prometheus sur le répertoire /etc/prometheus:

```bash
sudo chown -R prometheus:prometheus /etc/prometheus
```

Créer un fichier systemd pour promtheus afin de le configurer comme un service système linux (démarrage automatique au boot, gestion simplifié avec les commande systemctl, journalctl, paramètres d’exécution etc….) :

```bash
sudo nano /etc/systemd/system/prometheus.service
```

Ajouter la configuration suivante au fichier:

```bash
[Unit]
Description=Prometheus          # nom du logiciel
Wants=network-online.target     # voudrais démarrer une fois réseau opérationnel
After=network-online.target     # attend que le réseau soit opérationnel pour démarrer

[Service]
User=prometheus      # doit s'executer sous l'utilisateur prometheus
Group=prometheus     # doit s'executer sous le groupe prometheus
Type=simple
ExecStart=/etc/prometheus/prometheus \  # commande pour lancer prometheus
  --config.file=/etc/prometheus/prometheus.yml \     # fichier de conf principal
  --storage.tsdb.path=/etc/prometheus/data     # fichier ou sera stocké les données colectées

[Install]
WantedBy=multi-user.target
```

Recharger la configuration des services et démarrer Prometheus :

```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
```

Vérifier que Prometheus fonctionne correctement :

```bash
sudo systemctl status prometheus
```

Tester l’accès à l’interface web  en tapant dans le navigateur :

<aside>
📎

http://192.168.1.12:9090

</aside>

## Ajout de  flask-app à Prometheus

Pour ajouter notre application flask-app à notre Prometheus il faudrat ajouter une bibliothéque et modifier un peu le code de l’application (mise à jour sur git) :

requirements.txt : ajouter

```bash
pip install prometheus-flask-exporter
```

app.py

```bash
from flask import Flask
from prometheus_flask_exporter import PrometheusMetrics

app = Flask(__name__)
metrics = PrometheusMetrics(app)

@app.route('/')
def hello():
    return "Hello, DevOps World!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=3000)
```

Modifier le fichier suivant sur la vm ***flask-app*** :

```bash
sudo nano /etc/prometheus/prometheus.yml
```

Ajouter en dessous de ***scrape_configs***: 

```bash
scrape_configs:
  - job_name: 'flask-app'
    static_configs:
      - targets: ['localhost:1998']  
```

![image.png](image%2030.png)

Redémarrer Prometheus :

```bash
sudo systemctl restart prometheus
```

Vérifier que Prometheus Scrape les Metrics de l’Application :

- Retourner à l'interface web de Prometheus (`http://192.168.1.12:9090`)
- Clique sur **"Status"** > **"Targets"**

![image.png](image%2031.png)

- Tu devrais voir ton application (`flask-app`) listée comme une cible active. Le statut doit être "UP" s'il est correctement configuré.

![image.png](image%2032.png)

Après la configuration de métriques on va ajouter une alerte pour savoir quand l’application flask-app ne fonctionne plus :

Créer le fichier de configuration /etc/prometheus/alerts.yml et ajouter les lignes suivantes pour configurer l’alerte :

```bash
sudo nano /etc/prometheus/alerts.yml
```

```yaml
groups:
  - name: flask_app_alerts
    rules:
      - alert: FlaskAppDown
        expr: up{job="flask-app"} == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "L'application Flask ne répond plus"
          description: "L'instance de l'application Flask {{ $labels.instance }} ne répond plus depuis plus de 1 minute."
```

Ajouter ce fichier à la configuration Prometheus :

```bash
sudo nano /etc/prometheus/prometheus.yml
```

Ajouter le chemin du fichier :

```bash
rule_files:
  - '/etc/prometheus/alerts.yml'
```

![image.png](image%2033.png)

Redémarrer Prometheus :

```bash
sudo systemctl restart prometheus
```

On peut tester en stoppant le conteneur docker de l’application et sur Prometheus dans ***Alerts*** on devrais retrouver ceci :

![image.png](image%2034.png)

Si l’appli fonctionne bien l’alerte sera dans la section ***Inactive***

# Node exporter :

adresse: http://192.168.1.12:9100 

Node Exporter est un outil qui permet de collecter des **données de performance** d'un serveur (ou "nœud") pour les envoyer à Prometheus. 

Node exporter surveille **uniquement** le **système d’exploitation** et ses **ressources matérielles** (CPU, RAM, disque, etc.). Il ne fournit pas de métriques sur les **applications** ou **services** spécifiques comme une base de données ou un serveur web. pour cela il faut avoir des exporters spécifique.

## Installation node exporter :

doc Xavki:

***Il est recommandé de faire l’installation en binaire***

[https://gitlab.com/xavki/presentation-prometheus-grafana/-/blob/master/5-node-exporter/slides.md?ref_type=heads](https://gitlab.com/xavki/presentation-prometheus-grafana/-/blob/master/5-node-exporter/slides.md?ref_type=heads)

Tester  , dans le navigateur avec l’adresse :

```bash
http://192.168.1.12:9100/
```

Ajout du nœud node exporter dans Prometheus :

```bash
nano /etc/prometheus/prometheus.yml
```

Ajouter l’adresse du node exporter 192.168.1.12:9100 à la config :

***on à recréer un job_name pour le séparé de flask-app***

![image.png](image%2035.png)

Redémarrer Prometheus :

```bash
systemctl restart prometheus
```

En allant sur l’interface graphique de prometheus http://192.168.1.12:9090, dans l’onglet “**Targets**” , le nouveau nœud apparaîtra :

![image.png](image%2036.png)