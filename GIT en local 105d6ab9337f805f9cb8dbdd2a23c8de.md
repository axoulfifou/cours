# GIT en local:

### La gestion des versions :

Définition : Un **gestionnaire de version** est un système qui enregistre l’évolution d’un fichier ou d’un ensemble de fichiers au cours du temps de manière à ce qu’on puisse rappeler une version antérieure d’un fichier à tout moment

Github → stocké sur cloud qui appartient à Microsoft

Gitlab → stocké sur un serveur distant ou local

*Git Bash* → un émulateur de terminal

![image.png](image%2087.png)

*Git GUI* → une interface graphique

![image.png](image%2088.png)

### Infos commandes :

| niveau | argument | fichier | infos |
| --- | --- | --- | --- |
| système | --system | /etc/gitconfig | -- system signifie que ces paramètres s’appliquent à tous les utilisateurs de la machine et à tous les repo git |
| utilisateur | --global | ~/.gitconfig | --global signifie que ces paramètres s'appliquent à tous les dépôts Git sur votre machine.  |
| dépôt | --local | .git/config | -- local  signifie que ces paramètres s’appliquent seulement à ce repo git  |

### Ignorer des fichiers

⚠️ Avant même de créer notre premier projet Git, il est important d'avoir à l'esprit que **certains fichiers ne doivent pas être versionnés**

C'est en particulier le cas des fichiers de configuration, logs, build, etc ...

Le fichier [.gitignore](https://github.com/nicolas-sanch/versions-du-code-source/blob/main/1_Configuration/.gitignore) ( à la racine de notre projet) est utilisé pour indiquer à Git de ne pas prendre en compte certains fichiers

### **Les trois zones du flux de travail**

![image.png](image%2089.png)

- **Le répertoire de travail :** espace où sont effectué les modifications des fichiers.
    
    *exemple: 
    On modifie un fichier `main.py`. Tant que vous n'avez pas 
    ajouté ces changements à l'index (`git add`), Git sait que quelque chose a été 
    changé dans le répertoire de travail, mais ne l'a pas encore enregistré 
    pour la prochaine version*
    
- **L’index (Staging Area) :** C’est ici que vous "stocker" temporairement les fichiers modifiés ou ajoutés avant de les soumettre au dépôt. Pour ajouter un fichier à l’index :  `git add`
    
    *exemple : 
    Si on a modifié trois fichiers, mais qu’on veut en  enregistrer que deux dans votre prochaine version, on peut  ajouter les deux voulus à l'index avec `git add`*
    

- **Le dépôt (Repository):** endroit où les versions historiques de votre projet sont stockées. Pour faire le commit : `git commit`
    
    *exemple: 
    Après avoir ajouté les fichiers à l’index, la commande `git commit` les enregistre dans le dépôt, créant ainsi un nouveau point dans l’historique des versions.*
    
    ### Commandes:
    
    ```bash
    git init # Initialise un nouveau dépôt dans le répertoire courant
    git status # voir l'état des fichiers dans le dépôt
    git add -A # ajoute tous les fichiers modifiés et non indexés à l'index
    git commit -m "msg du commit" # Ajouter des fichiers au dépôt
    git rm <file> # supprime le fichier commité
    git restore --staged <file> # supprime le fichier de l'index (après un git add)
    git log # afficher tous les commits 
    git reflog # Log les commits et toutes les actions réalisées en local
    git blame <file> # identifier l'auteur du code 
    git revert <hash_du_commit> # permet de revenir au commit précédent (voir ordre commit avec git log)
    # cela va copier le commit précédent et créer un nouveau commit
    git reset <hash_du_commit> --hard # Permet de revenir à n'importe quel commit en supprimant tout ce qui est ultérieur
    git tag v2.1 # permet d'ajouter un tag ici v2.1
    git tag -a v1.0.0 -m "msg du tag" # Le -a indique qu'il s'agit d'un tag annoté
    git branch ma-branche      # Créer une nouvelle branche
    git checkout ma-branche    # Basculer sur ma-branche
    git merge <branche-source> # fusionne (attire) la branche-source pour la fusionner avec la branche actuelle
    git rebase <branche-source> # fusionne en créant les commits de la branche-source sur la branche actuelle
    git clean -f # supprime tous les fichiers non suivis avec git add
    ```
    
    **git status :**
    
    on peut voir l’état des  fichiers, ici on voit celui qui est  modifié mais qui à pas été suivis (avec `git add`)
    
    ![image.png](image%2090.png)
    

après avoir  `git add` :

![image.png](image%2091.png)

![image.png](image%2092.png)

**git revert :**

on va copier le commit précédent pour en refaire un nouveau commit et se positionner sur celui-ci :

![image.png](image%2093.png)

**Git reset :**

On revient simplement au commit précédent et on efface ce qu’il y a après

![image.png](image%2094.png)

**git tag :**

permet d’ajouter un tag pour se repérer pour savoir quelle version on a. On peu le voir en faisant `git log` :

![image.png](image%2095.png)

**git checkout :**

`git checkout <branche>` permet  de changer de branche

`git checkout <hash_commit>` permet de revenir à un commit voulu, mais on sort de la branche on travail plus sur une branche mais sur le commit en lui-même, pour revenir à la version la plus récente on fait `git checkout <branche>`

**git merge :** 

Fusionner deux branches pour intégrer les modifications d'une branche source dans une branche cible.

![image.png](image%2096.png)

**git rebase :**

Appliquer les commits d'une branche au-dessus d'une autre branche, créant un historique linéaire.

![image.png](image%2097.png)

### **Quand utiliser chaque approche ?**

- **Utilisez `git merge`** :
    - Lorsque vous voulez **préserver l'historique complet** et montrer les chemins parallèles pris par différentes branches.
    - Par exemple, dans des projets où plusieurs personnes travaillent sur des branches distinctes, et que vous voulez conserver une trace visuelle de ces développements dans l'historique.
- **Utilisez `git rebase`** :
    - Lorsque vous travaillez seul ou sur des branches de fonctionnalité et que vous voulez **nettoyer l'historique** avant d'intégrer votre travail dans `main` pour qu'il ait l'air plus propre et linéaire.
    - Par exemple, avant de fusionner une branche de fonctionnalité dans `main`, vous pouvez rebaser pour que l'historique semble simple et sans branches multiples.

### fork :

on clone un repo distant sur notre propre repo distant github 

on peut ensuite le modifier , l’améliorer etc comme on le souhaite

on peut ensuite faire un merge request pour proposer nos améliorations au repo de base où on à cloné par exemple. Utile pour améliorer les projets libre.