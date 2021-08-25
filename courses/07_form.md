# Les formulaires

## [Build](https://symfony.com/doc/current/forms.html)

La création de formulaire passe par la création d'une classe puis son exploitation.

### Make

Pour créer un formulaire. Il faut spécifier le nom de la classe et l'entity qui est utilisé par le formulaire.

```
bin/console make:form
```

### Build

Dans le controller, il faut construire le formulaire et la passer à la vue en créant une vue de formulaire.

-   Controller

```php
$entity = new Foo();
$form = $this->createForm(FooType::class, $entity);
return $this->render('foo/new.html.twig', [
    "form" => $form->createView()
]);
```

Si votre entity possède des classes en relation, vous aurez peut être un problème de conversion de l'instance au format chaine de caractère. Il y a plusieurs solutions, comme implémenter la méthode `__toString` dans la classe en relation. Il faut renvoyer l'attribut qui qualifie la valeur.

-   Entity

```php
public function __toString()
{
    return $this->name;
}
```

----------

### [Types](https://symfony.com/doc/current/reference/forms/types.html)

Chaque attribut de votre Entity peut posséder un élément HTML spécifiques. Par exemple un type password aura un style différent d'un type date. Il est possible de spécifier la valeur de l'attribut html type par champ du formulaire.

> La modifications des types se fait en second argument de la méthode `add` du builder.

-   Type

```php
$builder->add('email', EmailType::class)
```

Les types sont groupés par catégories.

-   Text Fields
-   Choice Fields
-   Date and Time Fields
-   Other Fields
-   Field Groups
-   Hidden Fields
-   Buttons
-   Base Fields

Pour chaque catégories il faut suivre la documentation et utiliser le type correctement.

Certains types attendent des options en troisième argument de `add`. Par exemple le type entity vous permet d'agréger une entité, un select se fait automatiquement.

```php
$builder
    ->add('bar', EntityType::class, [
        "class" => Bar::class,
        "multiple" => true,
        "expanded" => false
    ])
```

----------

### Request

Pour que le formulaire peuple l'entity des valeurs saisies, il faut fournir la request au formulaire. La Request peut s'obtenir en la déclarent en argument de l'action du Controller.

```php
public function new(Request $request)
{
    $entity = new Foo();
    $form = $this->createForm(FooType::class, $entity);
    $form->handleRequest($request);
```

----------

## [Display](https://symfony.com/doc/current/form/form_customization.html)

Pour afficher le formulaire, plusieurs functions sont disponibles dans twig.

### Start

Un formulaire doit s'ouvrir, `form_start` permet de créer l'ouverture du tag form.

```twig
{{ form_start(form) }}
```

Pour enlever la validation client il est possible d'ajouter des attributs html.

```twig
{{ form_start(form, { attr: { novalidate: 'novalidate' }}) }}
```

### End

Pour fermer la balise form il faut utiliser `form_end`.

```twig
{{ form_end(form) }}
```

### Row

Pour afficher le label, les erreurs, le widget et le help d'un élément de formulaire il faut utiliser la fonction `form_row`.

Il est possible de demander ce rendu pour l'ensemble du formulaire.

```twig
{{ form_widget(form) }}
```

Il est possible de cibler chaque élément du formulaire.

```twig
{{ form_widget(form.email) }}
```

### Label

Le label possède trois arguments.

Le second permet de surcharger le label.

```twig
{{ form_label(form.email, "Email") }}
```

Le troisème permet d'ajouter des attributs en utilisant label_attr.

```twig
{{ form_label(form.email, "Email", {
    label_attr: {
        class: 'my-class'
    }
}) }}
```

### Submit

Pour pouvoir réutiliser les formulaires, il est conseillé de fournir le submit dans la vue en utilisant un bouton.

```html
<button type="submit">Envoyer</button>
```

----------

## [Constraint](https://symfony.com/doc/current/validation.html#constraints)

Pour valider votre formulaire, vous devez contraindre vos champs à une valeur acceptable. La contrainte peut se spécifier aussi bien sur les Types que sur les Entities.

Les contraintes disponibles sont nombreuses, chaque documentation doit être étudiée avant de l'appliquer.

- Entity

```php
use Symfony\Component\Validator\Constraints as Assert;

class User
{
    /**
 * @Assert\NotBlank
 * @Assert\Length(min=3)
 */
    private $email;
}
```

De cette façon il est possible de prendre en compte les erreurs au niveau du formulaire qui se retrouve invalide.

- templates

```twig
{{ form_errors(form.email) }}
```

Quand votre formulaire est submit et valid vous souhaitez certainement étudier l'accès aux données.

- Controller

```php
$entity = new Foo();
$form = $this->createForm(FooType::class, $entity);
$form->handleRequest($request);
if ($form->isSubmitted() && $form->isValid()) {
    dump("Persist Entity");
}
return $this->render('foo/new.html.twig', [
    "form" => $form->createView()
]);
```

----------

[Retour au sommaire](00_sommaire.md)
