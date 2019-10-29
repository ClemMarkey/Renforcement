# Héritage

## Principe

Une classe peut hériter des propriétés et méthodes accessibles d'une classe "parente". La classe "enfant" peut alors utiliser les propriétés dont elle hérite, en plus de ses propres attribus et méthodes.

### `extends` : "hérite de"

```php
class Parent
{
  protected $attribut = "attribut du Parent";

  public function __construct()
  {
    echo "constructeur de la classe " . __CLASS__ . "<br>";
  }

  public function getAttribut()
  {
    return $this->attribut;
  }
}

class Enfant extends Parent
{
  public function childMethod()
  {
      echo "Methode de la classe " . __CLASS__ . "<br>";
  }
}

$objet = new Enfant;        // constructeur de la classe Parent
echo $objet->getAttribut(); // attribut du Parent
echo $objet->childMethod(); // Methode de la classe Enfant
```

Une classe ne peut hériter que d'une seule autre classe.  

`__CLASS__` : Renvoie le nom de la classe courante.


**Exemple**

Imaginons une classe **"Humain"** qui décrit les propriétés et méthodes d'un être humain (bras, jambe, marcher, etc...). Un être humain peut être un homme ou une femme par exemple, qui se partage les mêmes propriétés/méthodes (des bras, des jambes, etc.) mais qui ont aussi des différences :smirk: Plutôt que de créer des classes **"Homme"** et **"Femme"** en copiant/collant les propriétés/méthodes de la classe **"Humain"** et de modifier quelques informations, on va faire appel à l'héritage. On va dire : un **"Homme"** ben c'est un **"Humain"** (avec toutes ses propriétés/méthodes) avec des propriétés en plus.

Ainsi on peut créer une hiérarchie de classe comme cela :

- `class Humain { ... }`
- `class Homme extends Humain { ... }`
- `class Femme extends Humain { ... }`
- `class Fille extends Femme { ... }`

### `parent`

Ce mot clé permet d'appeler une méthode de la classe parente.  
Cela permet de préserver la fonctionnalité de la classe parente, tout en y ajoutant des fonctionnalités.

*Remarque: si le constructeur **n'est pas implémenté dans la classe fille**, le constructeur de la classe mère est automatiquement appelé à la création d'un objet de la classe fille.*

```php
class Parent
{
  const MYCONST = "Constante du parent";
  public function __construct()
  {
    echo "Constructeur de la classe " . __CLASS__;
  }
}

class Enfant extends Parent
{
  public function __construct()
  {
    parent::__construct(); // Appelle le constructeur de la classe parente
    echo "Constructeur de la classe " . __CLASS__;
  }
  public function parentConstant()
  {
    echo parent::MYCONST;
  }
}

// Constructeur de la classe Parent
// Constructeur de la classe Enfant
$objet = new Enfant;
// Constante du parent
$objet->parentConstant();
```

### `static` vs `self`

`self` fait référence à la classe courante (déjà vu plus haut), alors que `static` fait référence à la _classe-type_ de l'objet sur lequel est appelée la méthode.  
Si l'objet est du type de la classe courante, le traitement est identique. Mais si l'objet est d'une classe *héritant* de la classe courante, le résultat peut être différent selon les propriétés de la classe fille.

```php
class A
{
  public static $message = "Bonjour !";

  public static function hello() {
    echo self::$message;
  }
}

class B extends A
{
  public static $message = "Au revoir.";
}

A::hello();  // Bonjour !
B::hello();  // Bonjour !
```

```php
class A
{
  public static $message = "Bonjour !";

  public static function hello() {
      echo static::$message;
  }
}

class B extends A
{
  public static $message = "Au revoir.";
}

A::hello();  // Bonjour !
B::hello();  // Au revoir.

```

### Visibilité `protected`  
Les propriétés `protected` de la classe sont accessibles au sein de la classe et aux héritiers de celle-ci.  
Les propriétés `private` ne sont accessibles qu'au sein de la classe, pas de ses héritiers.  

```php
class Parent
{
  public $public        = "public";
  protected $protected  = "protected";
  private $private      = "private";

  function print()
  {
    echo $this->public . ",";
    echo $this->protected . ",";
    echo $this->private;
  }
}

$obj = new Parent;
echo $obj->public;      // public
echo $obj->protected;   // Fatal Error
echo $obj->private;     // Fatal Error
$obj->print();          // public, protected, private

class Enfant extends Parent
{
}
$childobj = new Enfant;
echo $childobj->public;      // public
echo $childobj->protected;   // Fatal Error
echo $childobj->private;     // Undefined property
$childobj->print();          // public, protected, undefined

```

### Niveaux d'héritage  
Il peut y avoir plusieurs niveaux d'héritage, les classes enfants ont accès aux propriétés de tous les niveaux supérieurs, les classes parentes n'ont pas accès aux propriétés des classes enfants.  

```php
class GrandParent
{
  public $propGrandParent = "Prop de class " . __CLASS__;
}

class Parent extends GrandParent
{
  public $propParent = "Prop de class " . __CLASS__;
}

class Enfant extends Parent
{
  public $propEnfant = "Prop de class " . __CLASS__;
}

$objGrandParent = new GrandParent;
echo $objGrandParent->propGrandParent; // Prop de class GrandParent
echo $objGrandParent->propParent;  // Undefined property
echo $objGrandParent->propEnfant; // Undefined property

$objParent = new Parent;
echo $objParent->propGrandParent; // Prop de class GrandParent
echo $objParent->propParent;  // Prop de class Parent
echo $objParent->propEnfant; // Undefined property

$objEnfant = new Enfant;
echo $objEnfant->propGrandParent; // Prop de class GrandParent
echo $objEnfant->propParent;  // Prop de class Parent
echo $objEnfant->propEnfant; // Prop de class Enfant
```

### Surcharge / Overriding
Les propriétés héritées de la classe mère peuvent être redéfinies dans la classe fille, et tenir compte de ses spécificités.

```php
class Parent
{
    public $public = "Public";

    // Visibilité non déclarée => public
    function print()
    {
        echo $this->public;
    }
    function method()
    {
        echo "parent method";
    }
}

class Enfant extends Parent
{
    // override property
    public $public = "Public child";

    // override method
    function method()
    {
        echo "child method";
    }
}

$obj = new Enfant;
$obj->print();      // Public child
$obj->method();     // child method
```

### `final`
* Une classe `final` ne peut être héritée par une autre classe.  
*ex: la class String de PHP est finale*  
```php
final class MyClass
{
    // ...
}

class MyOtherClass extends MyClass
{

} // Fatal Error
```

* Une méthode `final` ne peut être overridée: *pratique pour fixer un comportement obligatoire pour une classe qui peut être héritée.*
```php
class Parent
{
    public $public = "Public";

    public function print()
    {
        echo $this->public;
    }
    public final function method()
    {
        echo "parent method";
    }
}

class Enfant extends Parent
{
    // override property
    public $public = "Public child";

    // Fatal Error:
    function method()
    {
        // ...
    }
}
```

## Polymorphisme

Le *polymorphisme* est un concept de la programmation objet, qui signifie plusieurs formes, et désigne le fait que l’objet puisse avoir plusieurs formes. Cette capacité lui vient directement _du principe d’héritage_.

*Avantages : permet notamment la réutilisation du code - Don't Repeat Yourself*

Mais un objet a aussi la possibilité de redéfinir une méthode en la réécrivant complètement ou juste en la complétant on appelle ça **la surcharge**. Survient alors le principe de polymorphisme : choisir en fonction du contexte la méthode adaptée (selon que l'on s'adresse à _Homme_, _Femme_ ou _Fille_ par exemple).

### Abstraction de classes

* **Méthode abstraite**: méthode dont l'implémentation n'est pas faite dans la classe mère, mais qui le sera dans la classe fille.  
* **Classe abstraite**: classe qui contient au moins une méthode abstraite. Cette classe doit être déclarée abstraite et ne peut être instanciée.  

`abstract` : Une classe fille qui hérite d'une classe abstraite doit obligatoirement implémenter ces méthodes pour pouvoir être instanciée. L'implémentation d'une méthode abstraite dans une classe fille peut définir des paramètres optionnels.

```php
abstract class Restauration
{
    protected $adresse;

    // ne définit qu'un paramètre obligatoire
    abstract protected function servir($jour);
}

class Restaurant extends Restauration
{
    // la classe enfant peut définir des paramètres optionnels
    // non inclus dans la signature de la classe mère
    public function servir($jour, $menu)
    {
        // ...
    }
}

class Cantine extends Restauration
{
    public function servir($jour)
    {
        // ...
    }
}
```

### Interfaces

Si toutes les méthodes d'une classe sont abstraites, cette classe est une interface. Si une classe implémente une interface, elle doit obligatoirement implémenter toutes ses méthodes.

* Une interface permet donc de définir un comportement que doit obligatoirement fournir la classe qui l'implémente.  
* On la déclare avec le mot clé `interface` (le mot clé `abstract` est alors inutile), on la réalise/implémente avec `implements`.   
* Une classe peut implémenter plusieurs interfaces.  

Pour aller plus loin: [doc php sur les interfaces](http://php.net/manual/fr/language.oop5.interfaces.php)

```php
// Declarer l'interface "iTemplate"
interface iTemplate
{
    public function setVariable($name, $var);
    public function getHTML($template);
}

// Implémenter l'interface
class Template implements iTemplate
{
    private $vars = [];

    public function setVariable($name, $var)
    {
        $this->vars[$name] = $var;
    }

    public function getHTML($template)
    {
        foreach ($this->vars as $name => $value) {
            $template = str_replace('{' . $name . '}', $value, $template);
        }
        return $template;
    }

}
```

## Conclusion

On peut dire que la programmation orientée objet permet maintenant faire des programmes d’une taille et d’une complexité bien plus importantes qu'avec la programmation structurée.

Le principe orienté objet consiste à générer **comme une boîte noire contenant une donnée liée à un code** c’est ce que l’on appelle un objet. Cet objet est constitué d’attributs et de méthodes qui sont générée par un moule : **la classe**. Les éléments constituants cette classe sont nommés attributs et méthodes et peuvent être publiques ou privés : on parle **d'encapsulation**. Dans les objets il existe deux autres principes essentiels l'héritage et le polymorphisme. **L’héritage** consiste à récupérer les attributs des classes parentes pour éviter de les redéfinir. **Le polymorphisme** consiste à utiliser bonne méthode en fonction du type de l'objet.
