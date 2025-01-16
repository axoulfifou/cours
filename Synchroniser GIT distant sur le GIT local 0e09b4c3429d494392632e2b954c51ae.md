# Synchroniser GIT distant sur le GIT local :

```bash
git fetch --prune  # Met à jour les références locales et supprime celles qui n'existent plus sur le dépôt distant
git checkout main  # Passe à la branche main (ou une autre branche de ton choix)
git push origin --delete main  # Supprime la branche distante
git push origin main  # Pousse la branche locale vers le dépôt distant

# refaire l'opération si nécesaire pour une autre branche

```