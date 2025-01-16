# crontab

1. crontab –e sur rasp faire ‘’sudo nano /etc/crontab –e’’
2. on édit le fichier, on va devoir mettre une tâche par ligne et quand elle s’exécute.Chaque ligne sera composé de 6 colonnes (5 pour savoir quand elle s’exécute et 1 pour savoir laquelle)

| 1ere colonne | 2eme colonne | 3eme colonne | 4eme colonne | 5eme colonne | 6eme colonne |
| --- | --- | --- | --- | --- | --- |
| minutes | heures | Jours du mois (1er au 31) | Mois de l’année (1 au 12) | Jour de la semaine (
0 = dimanche
1 = lundi
2 = mardi
3 = mercredi
4 = jeudi
5 = vendredi
6 = samedi
7 = dimanche
(ça va du dimanche au dimanche) | commandes |

On met une étoile dans la colonne si on ne veut pas plus de précision.

Exemple : 5 * * * * ‘’commande’’ = toute les h +5 min la commande s’exécutera

On l’écrit à la suite des textes en #.