# Mise en place de l'environnement: Serveur VPN

Pour le vpn j’ai utilisé Tailscale , il faut l’installer sur chaque machines :

Sur windows : https://tailscale.com/download/windows

Sur linux (mon serveur proxmox) : curl -fsSL [https://tailscale.com/install.sh](https://tailscale.com/install.sh) | sh

Il suffit de se connecter au même compte (axoudu6@gmail.com), et une fois cela fait rechercher l’adresse ip du serveur proxmox , pour le moment  100.88.104.119 et de taper dans la barre de recherche [http://100.88.104.119:8006](http://100.88.104.119:8006) pour y accéder.

Sauf que avec ceci on à accès à l’interface proxmox mais pas aux interfaces graphique comme celle de GitLab par exemple. Il va donc falloir faire du serveur proxmox notre routeur :

## Utiliser Proxmox comme Routeur pour les VMs via Tailscale

Il faut d’abord configurer proxmox pour faire du routage, il faut décommenter cette ligne dans le fichier ***sysctl.conf:***

```bash
sudo nano /etc/sysctl.conf
```

![image.png](image%2013.png)

Appliquer ensuite la configuration :

```bash
sudo sysctl -p
```

### Configurer les Règles de Pare-feu :

Il faut autoriser le trafic entre l'interface Tailscale et le réseau local, pour cela on va ajouter des règles iptables:

```bash
sudo iptables -A FORWARD -i tailscale0 -o vmbr0 -j ACCEPT
sudo iptables -A FORWARD -i vmbr0 -o tailscale0 -j ACCEPT
# forward gérer les paquets qui doivent être routés à travers la machine 
# (c'est-à-dire les paquets qui ne sont pas destinés à la machine locale mais qui doivent passer d'une interface à une autre)
# comme ceux pour git qui doivent traverser proxmox pour aller sur la vm git

# -i tailscale0 : Cette règle spécifie que les paquets qui arrivent sur l'interface tailscale0 (c'est-à-dire ceux provenant du réseau Tailscale) sont concernés par cette règle.

# -o vmbr0 : Les paquets qui sont destinés à sortir via l'interface vmbr0 (le pont réseau de Proxmox, où se trouvent les VMs) doivent suivre cette règle.

# -j ACCEPT : Cette règle indique que les paquets qui répondent aux critères doivent être acceptés et donc transférés d'une interface à l'autre.
```

On va ensuite configurer Tailscale sur proxmox  pour qu'il annonce les routes vers ce sous-réseau (celui des vm) :

```bash
sudo tailscale up --advertise-routes=192.168.1.0/24

```

Proxmox à pour adresse 192.168.1.50 et les vm 192.168.1.XX mais les vm sont bien un **sous-réseau** de proxmox (via vmbr0) elle ne sont pas dans le même.

Il faut accepter cette route via l’interface graphique Tailscale :

connectez-vous et aller dans ***Admin console*** :

![image.png](image%2014.png)

Sur le host-002 (serveur proxmox) il doit y avoir un Subnet ! cliquer sur les 3 petits points à droite et sélectionner ***Edit route settings…*** :

![image.png](image%2015.png)

Cocher la case qui correspond à la route implémentée et cliquer sur ***save*** :

![image.png](image%2016.png)

Après cela le point d’exclamation de subnet doit avoir disparu.

### Désactiver l’expiration des clés :

Pour éviter que les clés concernant les machines expires et posent des problèmes de connexions nous allons désactiver  le délai.

Sur chaques machines cliquer sur les  3 petits points et sélectionner ***disable key expiry*** …

![image.png](image%2017.png)

une étiquette grise avec ***Expiry disabled*** doit apparaitre sous la machine après la manipulation.

### Persistance des Règles :

Pour rendre ces règles iptables persistantes après un redémarrage, on va utiliser utiliser   `iptables-persistent` :

Si lors de l’installation il demande la conservation des règles ipv4 et ipv6 actuelle sélectionner oui : 

```bash
sudo apt-get install iptables-persistent
```

Sauvegarder les règles au cas ou : 

```bash
sudo iptables-save > /etc/iptables/rules.v4
```

### Modifier le DNS :

Avec ce vpn le dns par défaut de tailscale sera utilisé, nos vm ne pourront alors plus faire la résolution de nom.

Dans la ***console admin*** de Tailscale, cliquer sur ***DNS*** :

![image.png](image%2018.png)

Puis dans la section ***Nameservers*** cliquer sur ***Add nameserver*** puis ***Custom…***

![image.png](image%2019.png)

Renseigner ensuite l’adresse de la box 192.168.1.254.

Sur le serveur proxmox redémarrer le service Tailscale :

```bash
sudo systemctl restart tailscaled
```

Sur l’interface graphique de proxmox cette fois-ci, ajouter dans la section dns l’adresse de la box:

*(dans le fichier /etc/resolv.conf, la config est réinitialiser au redémarrage)*

![image.png](image%2020.png)