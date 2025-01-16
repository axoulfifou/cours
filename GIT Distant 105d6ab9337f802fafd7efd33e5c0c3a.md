# GIT Distant :

```bash
git merge <ma-branche>                # Fusionne le contenu de la branche actuelle avec la branche dans master
git branch -d <ma-branch>             # Supprime la branche ma-branche (généralement réalisé après une fusion)
git push origin --delete <ma-branch>  # Supprime la branch ma-branche sur le répertoire distant
git clone path/to/local mon_depot # clone le dépôt git 
git push origin master # Envoyé les modifications sur la branche distante master
git pull # recevoir les modifications du dépot distant

```

```bash
# créer un répertoire lié à un repo distant
echo "# test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/xxxxx-xxxxxx/repo-example.git.  # Remplacer avec l'url de son projet
git push -u origin main
```

**git merge :**

Si les deux mêmes parties d'un fichier ont été modifié différemment, Git nous demandera de choisir entre les deux versions.

Nous utiliserons la commande `git status`pour visualiser quels fichiers sont *unmerged (les fichiers où il y a un problème au moment du merge)*

Après la résolution du conflit en éditant le fichier, il faudra ajouter cette résolution à l'index (`git add`) avant d'exécuter la commande `git commit` pour finaliser le commit de fusion.