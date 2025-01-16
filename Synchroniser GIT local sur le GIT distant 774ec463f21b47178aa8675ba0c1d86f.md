# Synchroniser GIT local sur le GIT distant :

Il faut s’assurer que ta branche actuelle correspond exactement à celle du dépôt distant.

```bash
git fetch --prune # Met à jour les référence locale et supprime celle qui n'existe plus sur le dépot distant
git checkout main # utilise la branche main
git reset --hard origin/main # aligne la branche locale avec la branche distante
git clean -fd  # supprime les fichiers et dossier non suivis
```