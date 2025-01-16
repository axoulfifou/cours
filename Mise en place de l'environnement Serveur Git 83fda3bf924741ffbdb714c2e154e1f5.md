# Mise en place de l'environnement: Serveur Git

**SERVEUR GITLAB :**

adresse : https://192.168.1.10

id: root

mdp: pikachu8787

1) installation de gitlab

```bash
sudo apt-get update   # Maj des paquets

sudo apt install curl openssh-server ca-certificates postfix # Install paquets

curl https://packages.GitLab.com/install/repositories/GitLab/GitLab-ee/script.deb.sh | sudo bash
# Ajout du repo « GitLab Package Repository »

sudo apt install GitLab-ce # Installationde  GitLab
```

il va ensuite falloir modifier le fichier gitlab.rb

```bash
cd /etc/gitlab
nano gitlab.rb
```

Il faut remplacer external_url par l’adresse ip de notre vm :

![image.png](image%209.png)

Et mettre cette ligne sur false pour corriger  les erreurs  lors de  l’installation:

![image.png](image%2010.png)

*Pour rechercher la ligne avec ctrl + w utiliser la console noVNC sur proxmox avec le ruban à gauche pour ctrl, sinon ctrl + w fermera la fenêtre.* 

Reconfigurer GitLab :

```bash
sudo gitlab-ctl reconfigure

```

Accéder à gitlab via [http://192.168.1.10/](http://192.168.1.10/) :

En haut à gauche sur le ***+*** sélectionner ***new project/repository*** :

![image.png](image%2011.png)

Puis ***create blank project*** :

![image.png](image%2012.png)

Puis configurer le project comme vous le souhaitez et ensuite lors de la création il créera automatiquement le [README.md](http://README.md) en initial commit.

## Mise en place du protocole SSL

La mise en place de ce protocole est nécessaire pour pouvoir faire un git clone car git n’accepte plus les url en http.

Sur la vm git il faut créer le répertoire ssl :

```bash
sudo mkdir -p /etc/gitlab/ssl
sudo chmod 700 /etc/gitlab/ssl
```

Générer un certificat SSL :

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/gitlab/ssl/gitlab.key -out /etc/gitlab/ssl/gitlab.crt
```

Configurer GitLab pour Utiliser le Certificat SSL :

```bash
sudo nano /etc/gitlab/gitlab.rb
```

modifier la ligne ***external_url*** en rajoutant un s et ajouter les lignes nginx :

```bash
external_url 'https://192.168.1.10'
nginx['redirect_http_to_https'] = true
nginx['ssl_certificate'] = "/etc/gitlab/ssl/gitlab.crt"
nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/gitlab.key"
```

Appliquer les modifications :

```bash
sudo gitlab-ctl reconfigure
```

On peut maintenant essayer de se connecter au git avec l’url en [https://192.168.1.10](https://192.168.1.10)