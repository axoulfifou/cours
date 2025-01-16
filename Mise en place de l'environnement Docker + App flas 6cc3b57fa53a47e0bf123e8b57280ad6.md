# Mise en place de l'environnement: Docker + App flask-app (locale)

Pour avancer dans le projet on va mettre en place une application de test basique que l’on manipulera par la suite avec les outils devops.

Pour utiliser docker sur windows on va utiliser docker desktop : 

[Documentation Docker pour l'installation sur Windows](https://docs.docker.com/desktop/install/windows-install/)

Ce lien fournit des instructions détaillées sur l'installation et la configuration de Docker Desktop sur Windows, y compris les prérequis système et les étapes à suivre.

Le détail de l’application se trouve sur le git.

Installer docker sur linux :

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