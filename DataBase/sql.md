# Le langage SQL

## Syntaxe - bases

### Commentaires
- `#` ou `--` pour une ligne  
- `/* ... */` pour plusieurs lignes

### DDL : Data Definition Language
Définition des données. **Cela correspond à la création des bases, tables, et champs**.
- CREATE (database, table, index)
- ADD (field, index, key)
- ALTER (database, table)
- MODIFY
- DROP
- TRUNCATE

### DQL : Data Query Language  
**Cela correspond à la manipulation des données**.  

#### Opérations de base: CRUD
**C**reate, **R**ead, **U**pdate, **D**elete (Ajout, Lecture, Modification, Suppression)


#### Lecture : `SELECT`
##### Depuis quelle table : `FROM`  

- Lire tous les champs d'une table :  
```
SELECT *
FROM nom_table
```

- Lire *des* champs spécifiques d'une table :  
```
SELECT champ1, champ2
FROM nom_table
```

- Attribuer un _alias_ à une table :  
```
SELECT p.name, p.price
FROM product p
```

##### Condition(s) : `WHERE`

- Lire des lignes de la table qui respectent une condition
```
# Tous les étudiants qui s'appellent Pierre
SELECT *
FROM students
WHERE first_name = "Pierre"
```

##### Conditions combinées : `AND`

- Lire des lignes de la table qui respectent plusieurs conditions
```
/* Tous les étudiants qui
    - s'appellent Pierre
    - sont nés après le 01/01/1989 */
SELECT *
FROM students
WHERE first_name = "Pierre"
AND birthdate > "1989-01-01";
```

##### Classement des résultats : `ORDER BY (ASC) (DESC)`

- Lire toutes les lignes et les classer par ordre alphabétique :
```
SELECT * FROM students
ORDER BY name
```
Par défaut, le sens du classement est `ASC` (ascendant, croissant), mais on peut également classer par ordre décroissant avec `DESC`.

- Lire les articles de blog, en les classant par date décroissante (les plus récents en premier) :
```
SELECT * FROM articles
ORDER BY publication_date DESC
```
On peut également mixer les classements, par exemple si on souhaite classer d'abord par nom, puis par prénom si plusieurs noms sont identiques :
```
SELECT * FROM students
ORDER BY name ASC,  first_name ASC
```

##### Jointures
- "Lier" 2 tables grâce à leur relation :
```
SELECT p.name, c.name
FROM product p, category c
WHERE p.category_id = c.category_id
```

- Autre syntaxe :
```
SELECT product.name, category.name
FROM product
LEFT JOIN category ON product.category_id = category.id
```
LEFT JOIN : prendre toutes les lignes de la première table, même s'il n'y a pas de valeur correspondante dans la deuxième (si _par exemple_, une catégorie associée à un produit a été supprimée).

### Création : `INSERT INTO ... VALUES`

- Insérer 1 ligne avec les valeurs pour chaque champ, dans l'ordre.
```
INSERT INTO schools VALUES
(2, "O'clock", "Everywhere");
```

- Insérer 1 ligne en omettant certaines valeurs. Provoquera une erreur si le champ est NOT NULL et pas AUTO_INCREMENT (A_I).
```
# student.id: AUTO_INCREMENT
# student.first_name: NOT NULL
# student.last_name: NULL
# student.birthdate: NOT NULL

INSERT INTO students (first_name, birthdate) VALUES
("Joe", "1980-10-01")
```

- Insérer plusieurs lignes :
```
INSERT INTO students (first_name, birthdate) VALUES
("Joe", "1980-10-01"),
("Jack", "1982-01-01"),
("John", "1981-01-10");
```

#### Modification : `UPDATE ... SET`
```
UPDATE nom_table
SET champ1 = "nouvelle valeur"
[WHERE condition]
```

#### Suppression : `DELETE`
```
# attention à la condition!
# s'il n'y en a pas, toutes les lignes sont supprimées sans condition.
DELETE FROM nom_table
WHERE condition
```

### Opérateurs et fonctions de base

#### Comparaison de chaînes : `LIKE`
Utilisé dans la clause `WHERE`, permet de rechercher les chaînes qui respectent un modèle.  
- Le caractère `%` est un joker, représente n'importe quelle chaîne de caractères.
```
# Etudiants dont le nom commence par un D
SELECT *
FROM students
WHERE last_name LIKE "D%";
```

#### Date courante

##### NOW()
`NOW()` renvoi 'la date et l'heure courante' en format `DATETIME`
```
# Lignes dont la date limite est dépassée
# (limit_date est de type DATETIME)
SELECT *
FROM table
WHERE limit_date < NOW();
```

##### CURDATE()
`CURDATE()` renvoi 'la date courante' en format `DATE`
```
# Lignes dont la date limite est dépassée
# (limit_date est de type DATE)
SELECT *
FROM table
WHERE limit_date < CURDATE();
```

##### DATE()
`DATE()` prend un `DATETIME` en paramètre et renvoi un format `DATE`
```
# Lignes dont la date limite est dépassée
# (limit_date est de type DATE)
SELECT *
FROM table
WHERE limit_date < DATE( NOW() );
```

#### Fonctions d'aggrégation
Les lignes peuvent être regroupées et éventuellement "aggrégées" grâce à la clause `GROUP BY`.  
Cela permet d'éxécuter certains calculs sur les lignes ainsi regroupées.


##### Nombre de lignes : COUNT()
```
# compte le nombre de lignes de la table products
SELECT COUNT(*)
FROM products
```

##### Addition : SUM()
```
SELECT person_id, person_name, SUM(tax_amount)
FROM taxes
GROUP BY person_id
```

##### Moyenne : AVG()
```
SELECT student_id, student_name, AVG(grade)
FROM grades
GROUP BY student_id
```