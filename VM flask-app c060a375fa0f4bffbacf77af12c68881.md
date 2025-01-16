# VM flask-app:

Cette vm proxmox sera utilisé comme serveur pour faire le déploiement de notre application flask-app.

adresse: http://192.168.1.12:1998

Le détail du code de l’application se trouve sur le git

commencer par installer sudo :

```bash
apt install sudo
```

mettre à jours les paquets :

```bash
sudo apt update
sudo apt upgrade
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