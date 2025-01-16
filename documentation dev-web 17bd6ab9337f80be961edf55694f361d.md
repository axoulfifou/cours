# documentation dev-web

1. Vérifier l’accès au repo git distant puis cloner ce dernier :

```bash
git clone <url_repo_git
```

1. Copier le fichier .env de notre environnement Laravel dans le dossier cloné si il n’y est pas :

```bash
cp .env <chemin_vers_le_dossier_cloné>
```

1. Lancer le gestionnaire **Composer** pour installer les dépendances dans le répertoire `vendor` de votre projet local 

```bash
docker run --rm --interactive --tty --volume $PWD:/app composer install
```

1. Lancer cette commande pour démarrer l’environnement Docker configuré spécialement pour  Laravel

```bash
./vendor/bin/sail up -d
```

1. On va ensuite faire la migration de donnée :

```bash
./vendor/bin/sail artisan migrate
```

⚠️ ATTENTION ! Il se peut qu’il y est une erreur du type :

```bash
Error response from daemon: driver failed programming external connectivity on endpoint blog_laravel-meilisearch-1 (118ec04adcb26925a699bf396931b2cbb553ec1b71ce5ee1d27cafa56b9e2085): Bind for 0.0.0.0:7700 failed: port is already allocated
```

Il faut aller dans le fichier .env et remplacer la variable APP_URL=http://localhost par APP_URL=http://127.0.0.1