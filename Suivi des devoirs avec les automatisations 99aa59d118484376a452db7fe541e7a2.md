# Suivi des devoirs avec les automatisations

<aside>
<img src="https://www.notion.so/icons/flash_yellow.svg" alt="https://www.notion.so/icons/flash_yellow.svg" width="40px" /> **Ce modèle utilise les automatisations des bases de données !**

Les automatisations de base de données vous permettent de mettre en place des déclencheurs et des actions basées sur des changements dans les bases de données. N’oubliez pas que les automatisations de base de données prennent quelques secondes à s’exécuter et qu’elles ne peuvent pas être modifiées, sauf si vous avez souscrit un plan Plus, Business ou Entreprise. [Pour en savoir plus sur les automatisations de base de données, cliquez ici](https://www.notion.so/fr-fr/help/database-automations).

- Automatisations utilisées dans ce modèle :
    - Dans la base de données `Devoirs`, quand un devoir est marqué comme `Soumis` → Indique la date et l’heure actuelle dans la propriété `Soumis le`.
    - Dans la base de données `Cours`, quand un nouveau cours est ajouté → Coche la case de la propriété `Actuellement inscrit(e)`.
</aside>

## Notes actuelles

[Sans titre](Sans%20titre%20a02d50657837459fa67a63859f8d0881.csv)

↓ Cette base de données utilise une automatisation pour définir automatiquement la propriété `Soumis le`. Dès que vous définissez le statut de votre travail comme étant `Soumis`, l'heure de soumission est fixée à maintenant !

[Devoirs](Devoirs%2084713dde7c35403dbe2c377d34680e4f.csv)

↓ Cette base de données utilise une automatisation pour cocher automatiquement la propriété "Actuellement inscrit(e)" chaque fois qu'un nouveau cours est ajouté.

[Cours](Cours%20e1e1a29712d54ab895ac5c022af252ea.csv)