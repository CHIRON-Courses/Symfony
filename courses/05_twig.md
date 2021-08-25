# [Twig](https://twig.symfony.com/)

Twig est le moteur de template utilisé par défaut de Symfony. Rapide, sécurisé et flexible, il permet de faire tout ce que fait PHP mais plus facilement.

## Render

Pour afficher une valeur dans un template il faut utiliser la méthode render et passer un tableau associatif. Les index du tableau correspondent aux variables disponibles dans le template.

```php
return $this->render('foo/index.html.twig', [
    'title' => "Hello World",
]);
```

## [Variables](https://twig.symfony.com/doc/3.x/templates.html#variables)

Pour afficher une variable il faut l’interpoler .

### Afficher

```twig
{{ title }}
```

Pour accéder aux attributs et méthodes il faut utiliser le point.

```twig
{{ foo.bar }}
```

### Créer

Il est possible de créer une variable.

```twig
{% set foo = 'foo' %}
```

----------

## [Filtres](https://twig.symfony.com/doc/3.x/filters/index.html)

Les variables peuvent être modifiées par des filtres. Les filtres sont séparés de la variable par un symbole de tuyau (|). Plusieurs filtres peuvent être chaînés. La sortie d'un filtre est appliquée au suivant.

```twig
{{ title|upper }}
```

> Symfony ajoute des filtres à Twig.

[Twig Filters](https://symfony.com/doc/current/reference/twig_reference.html#filters)

----------

## [Functions](https://twig.symfony.com/doc/2.x/functions/index.html)

```twig
{{ path('foo_index')}}
```

Les fonctions permettent d'obtenir une valeur de retour affichée dans le template.

> Symfony ajoute des fonctions à Twig.

[Twig Functions](https://symfony.com/doc/current/reference/twig_reference.html#functions)

----------

## [Include](https://twig.symfony.com/doc/3.x/tags/include.html)

```twig
{% include 'header.html.twig' %}
```

L'instruction include comprend un template et renvoie le contenu rendu de ce fichier.

> La convention de nommage pour les éléments inclus correspond à les préfixer par un _.

----------

## [Extends](https://twig.symfony.com/doc/3.x/tags/extends.html)

Définissons un template de base, base.html, qui définit un simple document squelette:

```twig
{% extends 'base.html.twig' %}
```

Pour inclure du contenu dans le squelette il faut utiliser la notion de bloc.

> La convention de nommage pour les éléments hérités correspond à les nommer layout.html.twig.

----------

## [Block](https://twig.symfony.com/doc/2.x/tags/block.html)

Les blocs sont utilisés pour l'héritage et agissent comme des espaces réservés et des remplacements en même temps.

Une fois que l'on hérite du template, il n'est pas possible de déclarer du contenu en dehors des blocs déclarés mais il est possible de créer de nouveaux blocs en dehors de blocs déclarés.

- Parent

```twig
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>{% block title %}Welcome!{% endblock %}</title>
        {% block stylesheets %}{% endblock %}
    </head>
    <body>
        {% block body %}{% endblock %}
        {% block javascripts %}{% endblock %}
    </body>
</html>
```

- Enfant

```twig
<!DOCTYPE html>
{% extends 'base.html.twig' %}

{% block body %}
    Contenu du block body
{% endblock %}
```

La fonction parent() récupère le contenu du bloc parent, utile pour surcharger une navigation ou un titre.

```twig
{% block title %}{{ parent() }}Foo{% endblock %}
```

----------

## [Condition](https://twig.symfony.com/doc/3.x/tags/if.html)

Le bloc `if` permet d'afficher sous condition.

```twig
{% if title == 'Hello' %}
    <h1>{{ title }}</h1>
{% endif %}
```

Pour la taille des tableaux, ils faut utiliser le filtre length.

```twig
{% if users  %}
    <h1>{{ users|length }}</h1>
{% endif %}
```

Pour l'existence d'une variable il faut utiliser is defined

```twig
{% if title is defined %}
    <h1>{{ title }}</h1>
{% endif %}
```

----------

## [Itération](https://twig.symfony.com/doc/2.x/tags/for.html)

Le bloc `for` permet d'itérer.

```twig
{% for user in users %}
    {{ user.username }}
{% endfor %}
```

### [Condition](https://twig.symfony.com/doc/2.x/tags/for.html#adding-a-condition)

Il est possible d'itérer et d'ajouter une condition pour chaque itération.

```twig
{% for user in users if user.active %}
    {{ user.username }}
{% endfor %}
```

----------

## [Webpack Encore](https://symfony.com/doc/current/frontend.html)

Avoir un système de template c'est bien, mais comment intégrer vos assets. Symfony propose une solution simplifiée pour l'utilisation de webpack.

- Installer Webpack Encore

```
composer require symfony/webpack-encore-bundle
```

- Télécharger les pakages front-end

```
npm install
```

- Configurer

Si vous souhaitez modifier les points d'entré, utiliser un pré-processor ou autre pre-built setting, rendez vous sur le fichier webpack.config.js et sur sa documentation.

- Utiliser

Pour utiliser Webpack, des scripts sont présents dans le package.json.

```
npm run watch
```

- Intégrer

Pour intégrer les fichiers qui sont générés dans "public/build", des extensions twig pour webpack-encore sont prévues afin de créer automatiquement les liens.

- Les balise link

```twig
{{ encore_entry_link_tags('app') }}
```

- Les balise script

```twig
{{ encore_entry_script_tags('app') }}
```

----------

[Retour au sommaire](00_sommaire.md)
