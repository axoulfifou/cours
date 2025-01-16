# Programmation orienté objet PYTHON

Des **classes** en découle des **objets**  (ex: la recette du gâteau au chocolat est la classe et le gâteau en chocolat est l’objet)

La préparation du gâteau elle s’appelle **l’instanciation de classe**

Les gâteaux préparés à partir de la même recette se ressemblent, c’est pareil avec les **objets** issus de la **même classe**, ils ont la **même structure**

Dans une classe il y a un **état** et un **comportement :**

état : **données** ou **variables**

comportement : série de chose que cette classe peut faire ⇒ ce comportement est contenue dans les **méthodes.** Une **méthode** fait partie d’une **classe** ou d’un **objet** contrairement à une **fonction**

| boîte à outils | marteaux | tournevis |
| --- | --- | --- |
| Etats **(Variables)** | Etats **(Variables)** | Etats **(Variables)** |
| nom outils (str) | couleurs marteau (string) | tailles (float) |
| nombre outils (int) | Comportements **(Méthodes)** | Comportements **(Méthodes)** |
| Comportements **(Méthodes)** | changer_couleur () | serrer_vis () |
| ajouter_outils () | enlever_clou () | desserer_vis () |
| supprimer_outils () | planter_clou () |  |

exemple de classe :

```python
	class Rectangle:  #nom de la classe
      width = 3
      height = 2       # Etats (variables)

      def calculate_area(self):    # méthodes (inclut tjr "self" en premier paramètre
           return self.width * self.height
```

Les constructeurs permettent de passer des vlauers initiales à un objet :

Example avec __init__ :

```python
class Rectangle:
    def __init__(self, length, width, color="red"):
        self.length = length
        self.width = width
        self.color = color
```

Pour l’appeler :

```python
 # Voici un exemple qui crée un  Rectangle de longueur 5 et de largeur 3 :

rectangle = Rectangle(5, 3)
```