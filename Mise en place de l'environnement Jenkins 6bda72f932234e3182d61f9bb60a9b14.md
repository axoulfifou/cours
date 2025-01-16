# Mise en place de l'environnement: Jenkins

adresse : http://192.168.1.11:8080

id: admin

mdp: pikachu8787

## Installation de jenkins:

Il faut commencer par installer GnuPG sur la vm afinde  gérer les clés GPG :

```bash
apt update
apt install gnupg -y
```

Ajouter la clé GPG à apt-key:

```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo gpg --dearmor -o /usr/share/keyrings/jenkins-archive-keyring.gpg
```

Ajouter le dépôt Jenkins :

```bash
echo "deb [signed-by=/usr/share/keyrings/jenkins-archive-keyring.gpg] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list
```

Mettre à jour les sources et installer Jenkins:

```bash
sudo apt update
sudo apt install jenkins -y
```

Il faut installer Java pour résoudre l’erreur lors de l’installation de  Jenkins:

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

Il faut trouver le chemin de java avec la commande :

```bash
sudo update-alternatives --config java
```

Le résultat attendu devrais être comme ceci :

![image.png](image%2021.png)

Copier le chemin et ajouter le  avec la ligne JAVA_HOME= dans le fichier /etc/default/jenkins :

![image.png](image%2022.png)

On peut ensuite accéder à l’interface graphique à l’adresse [http://192.168.1.11:8080](http://192.168.1.11:8080) , il faudra juste installer les composants recommandés. Le mdp de connexions se trouve dans le fichier /var/lib/jenkins/secrets/initialAdminPassword. Il est maintenant changé.

## Pipeline Jenkins :

Dans ***Nouveau Item*** saisir le nom et choisir le type ***Pipeline*** :

![image.png](image%2023.png)

Puis dans la section Pipeline renseigner le script ci-dessous :

```bash
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {                    # utilisation des variables des crédentials
                withCredentials([usernamePassword(credentialsId: 'git-credential', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh '''  # si repo exist on pull sinon on clone
                    if [ -d "projet-axel/.git" ]; then
                        cd projet-axel
                        git -c http.sslVerify=false pull # empeche la vérif de certificat
                    else
                        git -c http.sslVerify=false clone https://$GIT_USERNAME:$GIT_PASSWORD@192.168.1.10/root/projet-axel.git
                    fi
                    '''
                }
            }
        }      #  build de l'image depuis le dockerfile du git
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app -f projet-axel/Dockerfile projet-axel'
            }
        }      #  sauvegarde en.tar pour envois vers vm ou flask-app est déployée
        stage('Save Docker Image') {
            steps {
                sh 'docker save flask-app -o flask-app.tar'
            }
        }       # envois 
        stage('Transfer Docker Image') {
            steps {        # utilisation des variables des crédentials
                withCredentials([usernamePassword(credentialsId: 'ssh-credential', usernameVariable: 'SSH_USER', passwordVariable: 'SSH_PASS')]) {
                    sh '''
                    sshpass -p "$SSH_PASS" scp -o StrictHostKeyChecking=no flask-app.tar $SSH_USER@192.168.1.12:/tmp/
                    '''
                }
            }
        }              # stoper verison précédente et lancer la nouvelle
        stage('Deploy to Server') {
            steps {     # utilisation des variables des crédentials
                withCredentials([usernamePassword(credentialsId: 'ssh-credential', usernameVariable: 'SSH_USER', passwordVariable: 'SSH_PASS')]) {
                    sh '''
                    sshpass -p "$SSH_PASS" ssh -tt -o StrictHostKeyChecking=no $SSH_USER@192.168.1.12 '
                    docker load -i /tmp/flask-app.tar && \
                    docker stop flask-app || true && \
                    docker rm flask-app || true && \
                    docker run -d -p 1998:1998 --name flask-app flask-app
                    '
                    '''
                }
            }
        }
    }
}

```

Installer git sur la vm jenkins :

```bash
sudo apt install git
sudo systemctl restart jenkins
```

Installer docker :

```bash
## Suppression de la précédente version de Docker
sudo apt remove docker docker-engine docker.io containerd runc

## Mise en place du dépôt Docker
sudo apt install ca-certificates curl gnupg lsb-release
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

## Installation de docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Ajouter utilisateur jenkins au groupe docker pour pouvoir build et run :

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

## Installer plugin ***SSH agent*** et ***Pipeline Graph View***

cliquer sur ***Administrer Jenkins*** :

![image.png](image%2024.png)

Puis sur ***Plugins*** :

![image.png](image%2025.png)

Dans Plugins disponibles, sélectionner les plugins nécessaire, ***SSH agent*** et ***Pipeline Graph View*** cocher les et cliquer ***sur installer :***

![image.png](image%2026.png)

Redémarrer Jenkins :

```bash
sudo systemctl restart jenkins
```

Installer SSHpass : (pour éviter erreur lors du run pipeline)

```bash
sudo apt-get update
sudo apt-get install sshpass -y
```

## Ajouter les Credentials :

cliquer sur ***Administrer Jenkins*** :

![image.png](image%2024.png)

Puis sur ***Credentials*** :

![image.png](image%2027.png)

Puis ***(global)*** :

![image.png](image%2028.png)

Et ***+ Add Credentials*** :

![image.png](image%2029.png)

Il faut ensuite sélectionner ***Nom d’utilisateur et mot de passe*** et renseigner ces derniers pour la connexion ***ssh-credential*** et ***git-credential***. (ne pas se tromper sur la syntaxe de l’***ID*** car ils sont utilisé dans le script pipeline)