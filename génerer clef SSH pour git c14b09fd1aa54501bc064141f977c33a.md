# génerer clef SSH pour git

SSH = Protocole qui permet de faire une connexion sécurisée.

Pour créer une clé SSH :

1. Dans git bash : ssh-keygen -> refaire entrée plusieurs fois (si on obtient un dessin bizarre c’est bon).
2. Afficher la clé SSH : cat ~/.ssh/id_rsa.pub
3. Copie la clé puis dans le navigateur sur git hub , en haut à droite sur l’icone , setting -> SSH and GPG keys -> new SSH key -> donner un titre et coller la clé.
4. Créer un dossier sur le bureau puis Dans git bash : cd jusqu’au dossier créer -> git clone ‘’adresse SSH du repository’’