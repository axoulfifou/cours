# Elastic search

ip: [http://192.168.1.15:9200/](http://192.168.1.15:9200/)

Il faut que la vm est suffisamment de ressources :

![image.png](image%2055.png)

Télécharger le fichier elasticsearch:

```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.5.2.deb
```

Installer les dépendances nécessaires:

```bash
sudo apt install-y openjdk-17-jdk
```

Installer le package elasticsearch:

```bash
sudo dpkg -i elasticsearch-6.5.2.deb
```

Configurer le fichier de conf :

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

![image.png](image%2056.png)

**Il faut ensuite modifier ce fichier qui utilise des paramètres obsolètes:**

Commenter les 3 lignes de ce fichiers :

```bash
sudo nano /etc/elasticsearch/jvm.options
```

![image.png](image%2057.png)

et ajouter ces 3 lignes à la fin de ce même fichier

```bash
-XX:+UseG1GC
-XX:InitiatingHeapOccupancyPercent=75
-Djava.io.tmpdir=/tmp
```

![image.png](image%2058.png)

redémarrer elasticsearch

```bash
systemctl restart elasticsearch
```

ElasticSearch est  sur le port **9200**, on intéragit avec son API REST. Il
fonctionne principalement via des **requêtes HTTP REST** qui utilisent des outils comme `curl`, des clients HTTP (Postman, Insomnia), ou des bibliothèques spécifiques dans des langages de programmation (par exemple, Elasticsearch Python Client, ElasticSearch.js). 

Par exemple on peut envoyer une requête **GET pour voir l’état du cluster :**

```bash
curl -X GET "http://localhost:9200/"

```

![image.png](image%2059.png)

**Lister les index existants :**

*Les index sont en quelque sorte les dossiers dans lesquelles sont stockés et organisées nos données*

```bash
curl -X GET "http://192.168.1.15:9200/_cat/indices?v"
```

![image.png](image%2060.png)

*aucun pour le moment*

**Créer un index :**

```bash
curl -X PUT "http://192.168.1.15:9200/test_index"

```

![image.png](image%2061.png)

**Ajouter des documents :**

*Une fois l’index créé, vous pouvez y ajouter des documents. Un document est une unité de données (souvent en JSON) stockée dans un index*

```bash
curl -X POST "http://192.168.1.15:9200/test_index/_doc/1" -H 'Content-Type: application/json' -d'
{
  "name": "ElasticSearch",
  "type": "test",
  "description": "This is a test document",
  "created_at": "2024-12-22"
}'

```

![image.png](image%2062.png)

**Rechercher des documents dans un index :**

```bash
curl -X GET "http://192.168.1.15:9200/test_index/_search?q=ElasticSearch&pretty"
```

![image.png](image%2063.png)

**Supprimer un index :**

```bash
curl -X DELETE "http://192.168.1.15:9200/test_index"

```

L'utilité principale d'ElasticSearch n'est pas de créer manuellement des documents dans les index, mais plutôt de :

1. **Centraliser les données** (documents, logs, ou tout autre type d'informations).
2. **Effectuer des recherches rapides** sur ces données.
3. **Analyser et trier ces informations** en fonction des besoins spécifiques.