# Exercice POO

*Dans cet exercice, nous allons créer un petit programme de combat RPG*

Résultat attendu:
Dans le fichier `game.php`, grâce à la commande `monPerso->attaque(ennemi)` le programme doit calculer les points de vie restants à l'ennemi, basé sur les point d'attaque du personnage attaquant

## Step 1

- Dans un *namespace* global, créer une class `Personnage` dans laquelle vous mettrez les propriétés **privées** `PV`, `Ataque`.

- Dans un autre *namespace* appelé `Classe`, créer les class `Guerrier` ayant un *bonus de PV* de 10% et `Mage` qui aura un *bonus d'attaque* de 10%.

    **Rappel, une seule class par fichier**

*La **`Classe`** est la fonction de votre personnage, son rôle. A ne pas confondre avec les `class` qui servent à instancier des **objets***

Indice:
<details>
`Attila = new Guerrier()`
</details>

## Step 2

- Dans la class `Personnage`, ajouter la propriétés publique `attaque` 

Indice:
<details>
Connaître les notions de: Namespace, Méthodes publiques, Propriétés privées, Héritage de classe
</details>

## Step 3

- Créer une `class` `Monstre` héritant des propriétés de `Personnage`.
  - Votre `Personnage` doit attaquer son ennemi.
  - Le `Monstre` riposte à hauteur de 85% de son attaque de base.

Indice:
<details>
Entrée:

`Attila->attaque(Glorg)`

Sortie:

`Ouille ! Glrog a perdu x PV. Il lui reste x points de vie`

`Glorg se défend et inflige x Points de dégats à Attila. Il vous reste x points de vie`
</details>

## Step 4

- Quand les points d'un `Personnage` tombent à 0, la partie est finie.

---

*N'hésitez pas à poser des questions, proposer des améliorations ou bien même à aller loin dans vos délires !*

**Bisous**

<img src='../../img/kiss.gif'>