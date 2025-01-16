# hebergement web

Enoncée :

![image.png](image%202.png)

1. corrompre dns , avec les nom des serveurs (servername du virtualhost) lamp qui correspondent à l’adresse ip wan du pf sense
2. règle nat sur pfsensed pour autoriser requete a traverser le pfsense sur le port 80 et 443 pour quelle soit regdirigé sur l’adresse du reverse proxy
3. pour que le reverse proxy traite cette requete faut créer un virtualhost qui dira “il faut le rediriger ici” donc vers le serveur lamp 1 : en tout il en faudrat donc 4 ;  1 par url

server name :

seralias

faire le redirect vers 443 et paramétrer le 443 :

![image.png](image%203.png)

les ligne ssl sont pas obligatoires

dans proxypass et proxy pass reverse c’est l’ip du serveur lamp donc 

1. sur le serveur lamp on fait un virtualhost egalement : donc 2 sur chaque serveurs ; 2 et 2

![image.png](image%204.png)

on va utiliser le certificat autosigné lors de l’installation ssl apache mais pas besoin pour l’EI

Règle de par feu :

![image.png](image%205.png)

1. configurer le pare feu
- adresse lan et wan + passerelle
    
    test ping 
    

reverse proxy :

- configurer carte réseau + adresse  +test

![image.png](image%206.png)

configurer regle pare feu + check dns resolv.conf

- Installer apache + module reverse proxy
    
    créer les virtual hosts proxypass
    

proxypass vers web1

```bash
<VirtualHost *:80>
	ServerName web13il.fr
	ServerAlias www.web13il.fr
	ProxyPreserveHost On
	ProxyRequests Off
	ProxyPass / http://192.168.2.2/
	ProxyPassReverse / http://192.168.2.2/
</VirtualHost>

```

proxypass vers web2 :

serveur lamp 1: 

- configurer carte réseau + adresse  +test

installer lamp

installer les 2  versions de php fpm

## ajouter le dépot fpm

ajouter le dépot :

```bash
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list

```

ajouter la clé :

```bash
wget -qO - https://packages.sury.org/php/apt.gpg | sudo tee /etc/apt/trusted.gpg.d/php.gpg

```

Mettre a jour le dépot :

```bash
sudo apt update

```

```bash
sudo apt install php8.2 php8.2-fpm
sudo apt install php7.4 php7.4-fpm
```

ajouter le module pour que apache communique avec pgp -fpm :

```bash
sudo a2enmod proxy_fcgin  
```

créer les virtualhost :

```bash
<VirtualHost *:80>
    ServerName web11.fr
    ServerAlias www.web11.fr
    DocumentRoot /var/www/web11
    DirectoryIndex index.php    

    <Directory /var/www/web11>
        Options -Indexes +FollowSymLinks
        AllowOverride All
	Require all granted
    </Directory>
    
    <FilesMatch \.php$>
        SetHandler "proxy:unix:/var/run/php/php7.4-fpm.sock|fcgi://localhost/"
    </FilesMatch>
</VirtualHost>

```

créer les fichiers index.php dans /var

```bash
<?php
phpinfo();
?>
```

penser a donner les permissions:

Pour donne les droits aux page web php :

```bash
sudo chmod -R 755 /var/www/web1
sudo chown -R www-data:www-data /var/www/web1

```

si probleme découte ipv6 désactiver dans le grub

Pare feu :

rediriger les requêtes qu’ils reçois vers le reverse proxy :

![image.png](image%207.png)

mettre dans le fichier hosts de mon ordi le nom de l’url avec l’adresse ip du pare-feu en correspondance.

regle lan( pas obligatoire pour que sa marche mais nécessaire pour ping, ssh etc… le temps de l’install)

![image.png](image%208.png)