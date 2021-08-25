# [HTTP](https://symfony.com/doc/current/introduction/http_fundamentals.html)

La notion de route doit être acquise. Une route décrit la `requête` HTTP. Le Kernel du Framework va opérer le `matching` pour voir si la requête correspond à une route, puis le `dispatching` pour créer l'instance du `Controller` et des instances en dépendances pour finalement invoquer l'`action` du Controller, qui est la méthode ayant la responsabilité de fournir une `réponse`.

----------

# [Controller](https://symfony.com/doc/current/bundles/SensioGeneratorBundle/commands/generate_controller.html)

L'utilitaire Maker est disponible pour créer un controller et ses actions.

## Controller

-   Créer un controller

```
bin/console make:controller
```

```
bin/console make:controller Foo
```

La création d'un dossier dépend de votre environnement.

-   Unix

```
bin/console make:controller Foo\\Bar
```

-   Window

```
bin/console make:controller Foo\Bar
```

Vous constatez qu'un controller a été créé et que dans templates, un dossier correspondant à ses vues à été généré. Au dessus du controller vous constatez une annotation particulière.

### Annotation action

```php
/**
 * @Route("/foo", name="foo")
 */
public function index(): Response
```

Cela indique que l'action du controller doit être invoquée quand le chemin d'url correspond à "/foo". Si vous souhaitez une nouvelle action, vous pouvez la créer dans votre IDE et fournir une route en utilisant la même notation, attention, le chemin d'url et le nom de la route doit être unique.

### Annotation class

Dans un controller vous risquez d'avoir plus d'une action. Si toutes ces actions ont une base de chemin d'url commune, il est possible de spécifier cette base sur la classe et de compléter le chemin sur l'action. `Le nommage suivant est conseillé`.

```php
/**
 * @Route("/foo")
 */
class FooController extends AbstractController
{
    /**
 * @Route("/", name="foo_index")
 */
    public function index(): Response
```

----------

## Action

Par défaut l'action du controller est identifié par "index", la convention ne parle pas du nommage des actions, admettez les actions suivantes pour du CRUD.

```php
/**
 * @Route("/foo")
 */
class FooController extends AbstractController
```

-   Récupérer tous les items

```php
/**
 * @Route("/", name="foo_index")
 */
public function index
```

-   Créer un item

```php
/**
 * @Route("/new", name="foo_new")
 */
public function new
```

-   Récupérer un item

```php
/**
 * @Route("/{id}", name="foo_show")
 */
public function show
```

-   Modifier un item

```php
/**
 * @Route("/{id}/edit", name="foo_edit")
 */
public function edit
```

-   Supprimer un item

```php
/**
 * @Route("/{id}", name="foo_delete")
 */
public function delete
```

Ne retenez de ce listing que l'identifiant des actions, nous allons maintenant observer la notation des routes, les contraintes, les arguments et autre.

----------

# [Routes](https://symfony.com/doc/current/routing.html)

Une route est constitué de plusieurs attributs.

## Path

```php
/**
 * @Route("/foo")
 */
```

C'est la partie d'url après le host et le port. C'est ce qui permet de faire correspondre l'url avec une action.

### [Paramètre](https://symfony.com/doc/current/routing.html#route-parameters)

Un chemin peut avoir des paramètres.

Pour le chemin `/foo/7` l'action sera bien invoquée. L'identifiant du paramètre respecte les conventions de nommage des variables. Il est possible de déclarer plusieurs paramètres.

```php
/**
 * @Route("/foo/{id}", name="foo_index")
 */
public function index(): Response
```

Il est possible de récupérer la variable en la déclarant.

```php
/**
 * @Route("/foo/{id}", name="foo_index")
 */
public function index(int $id): Response
```

### [Paramètre optionnel](https://symfony.com/doc/current/routing.html#optional-parameters)

Il est possible de déclarer un paramètre optionnel, s'il est présent dans l'url ou non l'action sera invoquée.

```php
/**
 * @Route("/foo/{id}", name="foo_index")
 */
public function index(int $id = 1): Response
```

### [Contrainte](https://symfony.com/doc/current/routing.html#parameters-validation)

Il est possible de contraindre un paramètre en utilisant les expression régulières.

```php
/**
 * @Route("/foo/{id{id<[0-9]{1,3}>}}", name="foo_index")
 */
public function index(int $id): Response
```

----------

## Name

Bien qu'optionnel, il est important de déclarer une valeur à l'attribut name de la route avec comme convention de nommage, le nom du controller et celui de l'action en snake_case. Ce nom sera utiliser pour créer des liens par exemple.

----------

## Méthods

Il est possible de contraindre une méthode HTTP sur une route.

```php
/**
 * @Route("/new", name="foo_new", methods={"GET","POST"})
 */
```

----------

## Autre

Il est possible de contraindre sur la valeur d'une entête, un nom de domaine et autre!

----------

# Debug

La bonne pratique correspond à créer des routes en annotations. Pour lister les routes il y a un utilitaire de debug.

```
bin/console debug:route
```

----------

[Retour au sommaire](00_sommaire.md)
