# Système de Gestion de Bases de Données Relationnelles

> SGBDR = Relational DataBase Manager System (RDBMS) en anglais.

## Les tables

- Une table appartient à une base données.
- Elle est identifiée par son nom.
- Colonnes = champs de la table (nom & type).
- Lignes = enregistrements de la tables, **uniques**.

### Exemple, table _computer_

reference  | name            | price  | description  | category  
------- | --------------- | ------ | ------------ | --------  
PC-013HP|HP 14-r206 Intel | 255.30 | Processeur...| pc
T-838AR |Archos 50 Neon   |  98.27 | Ecran 5"HD...| phone
PC-016HP|HP 14-t408 Intel | 480.30 | Processeur...| pc

## Principaux types de données

La liste complète des [types de données](https://dev.mysql.com/doc/refman/5.7/en/data-type-overview.html).

Pour rappel 1 `byte` = 1 `octet` = 256 valeurs.

### Nombres

- UNSIGNED sans signe : 128
- SIGNED avec signe  : -128

Nature | Type SQL | UNSIGNED | SIGNED | Taille (octet)
------------|----------|----------|:-- |--------------:
Entiers | `TINYINT` | `[0 à 255]` | `[-128 à 127]`  | 1
Entiers | `SMALLINT` | `[0 à 65,535]` | `[-32,768 à 32,767]` | 2
Entiers | `MEDIUMINT` | `[0 à 16,777,215]` | `[-8,388,608 à 8,388,607]` | 3
Entiers | `INT` | `[0 à 4,294,967,295]` | `[-2,147,483,648 à 2,147,483,647]` | 4
Entiers | `BIGINT` | `[0 à 18,446,744,073,709,551,615]` | `[-9,223,372,036,854,775,808 à 9,223,372,036,854,775,807]` | 8
Flottants | `FLOAT` | `[0 à 3.402823466E+38]` | `[-1.175494351E-38 à 3.402823466E+38]` | 4

### Caractères

Nature | Type SQL |  Note |  Taille (octet)
------------|----------|:-- |--------------:
Chaîne de caractères | `VARCHAR` | Peut contenir jusqu'à 255 caractères | Longeur de la chaîne maximum + 1
Chaîne de caractères | `CHAR` | Maximum de 255 caractères, idéal pour stocker des valeurs de taille systématiquement identique | Longueur de la chaîne
Chaîne de caractères | `TEXT` | Chaînes de 65,535 caractères, sensible à la casse | Longueur + 2
Chaîne binaire (séquence d'octets)| `BLOB` | 65,535 caractères, insensible à la casse | Longueur + 1

### Dates

Nature | Type SQL |  Note |  Taille (octet)
------------|----------|:-- |--------------:
Date au format <br> `AAAA-MM-JJ` | `DATE` | De `1000-01-01` à `9999-12-31` | 3
Date et heure au format <br> `AAAA-MM-JJ HH:MM:SS` | `DATETIME` | De `1000-01-01 00:00:00` à `9999-12-31 23:59:59` | 8
Heure au format `HH:MM:SS` | `TIME` | De `-838:59:59` à `838:59:59` | 3
Année à 2 ou 4 chiffres | `YEAR` | De `1901` à `2155` ou `0000` (4 chiffres) | 1
Date sous forme numérique | `TIMESTAMP` | Commence le `1970-01-01 00:00:00` |

---

## INDEX

### INDEX
Permet d'ajouter une référence à un champ qui rend son indexation plus efficace et améliore la vitesse de recherche de MySQL pour ce champ.

### UNIQUE
Permet de déclarer un champ pour lequel toutes les valeurs devront être unique. Par exemple un champ `email` *UNIQUE* permet de garantir l'unicité d'une adresse email enregistrée.

### PRIMARY
Permet d'identifier sans équivoque un enregistrement. Doit être `UNIQUE` et `NOT NULL`. Très souvent (mais pas obligatoire) associé à un attribut **AI** *(Auto Increment)*  sur un champ fréquemment nommé `id`.

---

**PRIMARY KEY** : **Clé primaire** permet d'identifier sans équivoque un enregistrement

**FOREIGN KEY** : **Clé étrangère** référence à une clé d'une autre table

**AI** : **Auto Increment** permet d'automatiquement définir une valeur numérique partant de 1 et en l'incrémentant à chaque enregistrement. Premier enregistrement `1`, 2ème enregistrement `2`, etc... pour tous les enregistrements suivants.

---

## Relations

Partant de cet exemple simple de boutique :

> Les **clients** possèdent une **fiche client** avec leur adresse de facturation et de livraison ainsi que leur date de naissance
>
> Les **clients** peuvent passser des **commandes**
>
> Les **commandes** possèdent un prix, une date et un listing de tous les **produits** qu'elles contiennent
>
> Les **produits** possèdent un nom et un prix

![schema boutique](img/schema_boutique.png)

### 1:1 *one to one*

1 **client** possède 1 seule **fiche client** et 1 **fiche client** ne correspond qu'à 1 **client**

> Le lien entre notre **client** et notre **fiche client** est assuré par une clé étrangère dans la table `clients` grâce au champ `clients.fiche_id` faisant référence à la table `fiches_client` et au champ `fiches_client.id`

<img src="img/1-1.png" alt="1:1" width="400">

### 1:n *one to many*

1 **client** peut passer *n* (plusieurs) **commandes** et plusieurs **commandes** peuvent être passées par le même **client**

> Le lien entre notre **client** et ses **commandes** est assuré par une clé étrangère dans la table `commandes` grâce au champ `commandes.client_id` faisant référence à la table `client` et au champ `clients.id`


<img src="img/1-n.png" alt="1:n" width="200">


### n:n *many to many*

*n* (plusieurs) **produits** peuvent appartenir à *n* **commandes** et *n* (plusieurs) **commandes** peuvent avoir *n* (plusieurs) **produits**

> Le lien entre nos **commandes** et ses **produits** est assuré par une  table d'association `commande_produit` qui va regrouper des clés étrangères. `produit_id` fait référence à la table `produit` et au champ `produit.id` et `commande_id` fait référence à la table `commandes` et au champ `commande.id`

<img src="img/n-n.png" alt="n:n" width="600">

---

## Outils de conception et de visualisation

### MCD (Conceptuel)

#### [AnalyseSI](https://github.com/AnalyseSI/AnalyseSI) ( mac / win / linux )

Outil JAVA de conception visuelle.

#### [Looping MCD](http://www.looping-mcd.fr/) ( win / linux & mac  + wine )

Outil de conception visuelle. Fonctionne très bien avec Wine sous Linux ou Mac.

### MLD/MPD (Logique et Physique)

#### [MySQL Workbench](https://www.mysql.fr/products/workbench/) ( mac / win / linux )

Outil de conception très complet, à utiliser surtout pour les bases de données complexes.

#### [Sequel PRO](https://sequelpro.com/) ( mac )

Outil de gestion de bases de données.

#### [Heidi SQL](https://www.heidisql.com/) ( win )

Outil de gestion de bases de données.

#### [phpMyAdmin](https://www.phpmyadmin.net/) ( web )