# Ajouter_supprimer QR code

1. Dans putty se connecter aux 2 rasps (sas et escalier)
2. Id = mdp=
3. cd opencv-2.4.9/
4. sudo nano BDD.txt
5. Dans ce fichier on enlève ou ajoute les id.

![image1%203.png](image1%203.png)

**Ajouter**

= Vérifier si il existe déjà avec Ctrl+w si il existe pas on ajoute l’id à la fin de la liste et on pense à laisser une ligne vierge dessous.

**Supprimer** = On recherche l’id à supprimer avec Ctril+w puis on l’efface comme avec un éditeur de texte classique ‘’suppr’’.

1. Pour quitter, faire Ctrl+x, puis ‘’o’’ pour enregistrer les modifications et ‘’entrer’’ pour valider le nom de fichier.
2. Aller sur filezilla se connecter en sftp sur chaque rasp puis dans opencv-2.4.9 chercher le fichier BDD.txt.
3. Clique droit -> télécharger, il faut récupérer les fichiers BDD.txt de cette manière sur chaque rasps (escalier + sas)
4. Enregistrer ces fichiers dans teams , dans FT – MDW TEAM -> Sécurisation Site Limoges -> sauvegardes ->(choisir SAS ou escalier en fonction du rasp en question) -> BDD -> enregistrer ici au format BDD_année_mois_jours (tout en chiffre).