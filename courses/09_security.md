# Sécurité

Le concept de sécurité fait référence au composant de sécurité qui gère les authentification et autorisations des utilisateurs.

----------

## [User](https://symfony.com/doc/current/security.html#a-create-your-user-class)

Pour manipuler un utilisateur qui peut être manipulé par le composant security, il faut le générer avec le maker.

```
bin/console make:user
```

Un utilisateur est fait pour être authentifié et Symfony vous permet de générer cette authentification, sa configuration, son controller, sa vue.

----------

## [Login](https://symfony.com/doc/current/security/form_login_setup.html#generating-the-login-form)

Pour générer un login/logout il faut générer une authentification.

```
bin/console make:auth
```

Comme suggéré par le CLI, il vous faut éditer votre `Authenticator` pour rediriger votre utilisateur sur une route de votre choix après un success d'authentification, en décommettant la redirection dans `onAuthenticationSuccess` et en modifiant le nom de la route en argument.

### Custom

Si vous pensiez créez vous mêmes votre login/logout vous n'aurez pas cette opportunité.

> Si vous modifiez le Controller généré pour qu'il respecte votre convention pour le nom des routes vous devrez modifier la constante `LOGIN_ROUTE` de l'`Authenticator`, ainsi que le nom de la route logout dans le fichier `config/packages/security.yml`. Empêchez un utilisateur d'accéder à la vue s'il est connecté en décommettant l'instruction en rapport puis personnalisez le template.

### Signup

En revanche pour utiliser le login il faut posséder dans la base de données un utilisateur avec un mot de passe haché avec une implémentation de `UserPasswordEncoderInterface`. il est possible de générer votre création de compte avec `make:registration-form`.

----------

## [Roles](https://symfony.com/doc/current/security.html#a-create-your-user-class)

Il est possible de restreindre l'accès à des actions en fonction du rôle possédé.

### [Annotation](https://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/annotations/security.html)

Chaque utilisateur possède au moins un `ROLE_USER`. Un utilisateur anonyme possède le rôle `IS_ANONYMOUS`.

> Le nouveau système d'authentification n'accorde plus le role d'anonyme au visiteur non-connecté. Ces sessions sont maintenant traité comme non-authentifié. Lorsque vous utiliserez `isGranted()`, le résultat sera toujours `false` vu que cette session sera considéré comme un utilisateur ne possédant aucun privilège.
> 
> Vous devrez utiliser le nouvel attribut de sécurité `PUBLIC_ACCESS`, dans `access_control` du fichier de configuration des sécurités, pour autoriser les utilisateurs non-authentifié à accèder aux routes (comme la page de connexion).

Il est possible avec avec une annotation de rediriger l'utilisateur vers la page de login selon l'exemple suivant.

```php
use Sensio\Bundle\FrameworkExtraBundle\Configuration\IsGranted;

/**
 * @IsGranted("ROLE_USER")
 * @Route("/foo")
 */
```

> L'annotation `@Security` offre une granularité plus fine sur les rôles en l’absence de hiérarchie de rôles définie dans security.yml.

### Programmation

Il est possible de vérifier si un utilisateur est connecté.

```php
if ($this->getUser()) {
    return $this->redirectToRoute('some_route');
}
```

Il est possible de vérifier son rôle.

```php
if (!$this->isGranted("ROLE_USER")) {
    return $this->redirectToRoute('some_route');
}
```

Il est possible de lever une exception si son rôle ne correspond pas.

```php
$this->denyAccessUnlessGranted('ROLE_USER');
```

### [Twig](https://symfony.com/doc/current/reference/twig_reference.html#is-granted)

Il est possible de vérifier le rôle dans la vue pour modifier la logique d'affichage.

```twig
{% if is_granted("ROLE_USER") %}
    <a href="{{ path('security_logout') }}">
        <strong>Logout</strong>
    </a>
{% else %}
```

----------

[Retour au sommaire](00_sommaire.md)
