# Namespaces

Namespaces ou espaces de noms permettent de résoudre 2 principaux problèmes :

1. Collisions de noms entre le code que vous créez, les classes, fonctions ou constantes internes de PHP, ou celle de bibliothèques tierces.
2. La capacité de faire des alias ou de raccourcir des Noms_Extremement_Long pour aider à la résolution du premier problème, et améliorer la lisibilité du code.

Par défaut, nous sommes dans l'espace de [nom global](https://secure.php.net/manual/fr/language.namespaces.global.php) de PHP.

## Déclarer un namespace

Le mot clé `namespace` permet de déclarer un espace de nom.

Quand une namespace est déclaré, il doit systématiquement être le premier élément d'un script.

```
<?php
namespace ProjetGenial;

class App { /* ... */ }
function configure() { /* ... */ }

```

## utiliser un namespace

En supposant qu'une classe `Dbmanager` possède un namespace `ProjetGenial\Util`

### Par [référence](https://secure.php.net/manual/fr/language.namespaces.basics.php)

```
<?php

$manager = new ProjetGenial\Util\Dbmanager;

```

### Par [importation et alias](https://secure.php.net/manual/fr/language.namespaces.importing.php)

```
<?php
use ProjetGenial\Util\Dbmanager;

$manager = new Dbmanager;

```

Il est possible de redéfinir le nom (alias en cas de noms identiques par exemple)

```
<?php
use ProjetGenial\Util\Dbmanager as DB;

$manager = new DB;

```

## PSR-4

[The PHP Framework Interop Group (php-fig)](http://www.php-fig.org/) est un groupe de travail et de réflexion réunissant des acteurs majeurs de l'écosystème PHP.

Ce groupe de travail à établi plusieurs recommandations listées dans les [PHP Standards Recommandations (PSR)](http://www.php-fig.org/psr/)

La recommandation [PSR-4](http://www.php-fig.org/psr/psr-4/) succède à la PSR-0 qui est dorénavant dépréciée, cette recommandation établie la marche à suivre pour gérer l'**autoload** de classes réparties dans différents **namespace**.

Pour simplifier la recommandation : "Les namespaces doivent reflétés l'arborescence des dossiers/fichiers" et "Le nom du fichier de classe doit être identique au nom de la classe elle-même + `.php`"

```
 \<NamespaceName>(\<SubNamespaceNames>)*\<ClassName>
```

 | Fully Qualified Class Name   | Namespace Prefix | Base Directory         | Resulting File Path                       |
 |------------------------------|------------------|------------------------|-------------------------------------------|
 | \Acme\Log\Writer\File_Writer | Acme\Log\Writer  | ./acme-log-writer/lib/ | ./acme-log-writer/lib/File_Writer.php     |
 | \Aura\Web\Response\Status    | Aura\Web         | /path/to/aura-web/src/ | /path/to/aura-web/src/Response/Status.php |
 | \Symfony\Core\Request        | Symfony\Core     | ./vendor/Symfony/Core/ | ./vendor/Symfony/Core/Request.php         |
 | \Zend\Acl                    | Zend             | /usr/includes/Zend/    | /usr/includes/Zend/Acl.php                |


## Configurer la PSR-4 avec composer

Voir [Fiche récap > PHP > Composer](composer.md)