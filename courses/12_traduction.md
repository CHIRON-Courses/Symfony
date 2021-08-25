# Traduction

## [Locale](https://symfony.com/doc/current/translation/locale.html#the-locale-and-the-url)

Vous pouvez définir la langue locale par défaut dans `config/packages/translation.yaml`.

```yaml
framework:
    default_locale: en
```

Au lieu de forcer une valeur de la langue locale, vous pouvez définir la langue locale qui sera utilisé si celle de l'utilisateur est introuvable.

```yaml
framework:
    translator:
        fallbacks: ['en']
```

----------

## [Fichier](https://symfony.com/doc/current/translation.html#translation-resource-file-names-and-locations)

Les fichiers de traduction sont placés dans `translations`. Vous pouvez stocker des fichiers par domain et par langue.

-   translations/messages.en.yaml

```yaml
user:
  pseudo: Pseudo
  email: Email
  password: Password
  confirm: Confirmation
```

-   translations/validators.en.yaml

```yaml
user:
  confirm: The confirmation must match
```

-   translations/errors.en.yaml

```yaml
user:
  exists: Account already exists
```

----------

## [Utilisation](https://symfony.com/doc/current/translation/templates.html#using-twig-filters)

Pour utiliser vos traductions vous remarquerez que les formulaires traduisent automatiquement les messages de label en utilisant le domain "messages" et d'erreur en utilisant le domain "validators".

Dans le cadre d'un controller, il faut utiliser l'autowiring.

```php
$error = $translator->trans("user.exists", [], "errors"));
```

Dans twig il faut utiliser le filtre trans.

```twig
{{ error|trans }}
```

----------

[Retour au sommaire](00_sommaire.md)
