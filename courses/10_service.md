# Service

> Les services s'occupent de la logique métier

----------

## [Autowire](https://symfony.com/doc/current/service_container.html)

A partir de Symfony 4, toutes les classes de `src/` sont des services. C'est à dire que sans aucune configuration il est possible d'en obtenir une instance en déclarant son type en paramètre.

Pour connaitre les services des classes Symfony disponible il faut demander un debug de la fonctionnalité autowiring.

```
$ bin/console debug:autowiring
```

Pour déclarer un service il faut créer une classe manuellement, elle est disponible à la déclaration dans un constructeur ou une action et peut lui même utiliser l'autowiring dans son constructeur.

-   Service

```php
class UserService
{

    private UserRepository $repository;

    public function __construct(UserRepository $repository)
    {
        $this->repository = $repository;
    }

}
```

-   Action

```php
public function new(UserService $userService): Response
```

> Mais quoi mettre dans les services?

Les services possèdent la logique métier. Vous ne devriez pas, directement dans le controller, effectuer de la logique métier réutilisable, et utiliser les services pour le faire. Les accès aux données, transformation de format, construction de tris et autre logique doit être externalisée dans les services.

----------

## [Arguments](https://symfony.com/doc/current/service_container.html#manually-wiring-arguments)

Il est possible que vous souhaitiez injecter une valeur primitive dans un service, pour ce faire vous devez utiliser le fichier de configuration du service.

Les services sont configurés par défaut dans `config/services.yml`, il est possible d'organiser les fichiers de configuration de services. La syntaxe pour injecter un argument primitif est la suivante dans ce fichier.

-   config/services.yaml

```yaml
App\Service\SomeService:
    arguments:
        $email: 'manager@example.com'
```

-   Service

```php
class SomeService
{

    private string $email;

    public function __construct(string $email)
    {
        $this->email = $email;
    }

}
```

----------

## [Parameters](https://symfony.com/doc/4.1/service_container/parameters.html)

Pour stocker vos variables d'environnement et autre valeurs liés à la configuration vous disposer des fichiers `.env` et `config/services.yaml`.

### [Retrieve](https://symfony.com/doc/4.1/service_container/parameters.html#getting-and-setting-container-parameters-in-php)

Une variable d’environnement peut se situer dans `.env`.

-   .env

```
API_KEY=my_secret_key
```

La fonction getenv ne charge plus par défaut les variables d'environnement, il est préférable de passer par les services. Vous pouvez créer un paramètre qui lui a accès aux variables d'environnement.

-   config/services.yaml

```yaml
parameters:
    api.key: '%env(API_KEY)%'
```

Pour la récupérer dans un controller.

-   SomeController

```php
$this->getParameter("api.key")
```

Pour la récupérer dans un service.

-   services.yaml

```yaml
App\Service\ApiService:
    arguments:
        $key: '%env(API_KEY)%'
```

Pour récupérer un paramètre.

-   services.yaml

```yaml
App\Service\ApiService:
    arguments:
        $key: '%api.key%'
```

### [Organiser](https://symfony.com/doc/current/service_container/import.html)

Vous pouvez déclarer des fichiers de services dédiés.

-   config/services/api.yaml

```yaml
parameters:
    api.key: '%env(API_KEY)%'
```

Et ensuite les importer dans le point d'entré des services.

-   config/services.yaml

```yaml
imports:
    - { resource: services/api.yaml }
```

----------

[Retour au sommaire](00_sommaire.md)
