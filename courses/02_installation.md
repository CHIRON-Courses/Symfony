# Installation

Nous allons installer la dernière version bien que n'étant pas la LTS, il y a peu de changements entre la LTS et la latest. Il est possible d'installer Symfony avec Composer ou avec le birary Symfony.

## [CLI](https://symfony.com/download)

Tous les packages peuvent s'installer avec le package manager. Cependant le framework propose un binary pour ce faire.

- Créér un projet

```
symfony new my_project_name --full
```

- Créer un projet pour une version spécifique

```
symfony new my_project_name --full --version=4.4
```

- Démarrer le serveur

```
symfony server:start
```

- Vérifier les pré-requis

```
symfony check:requirements
```

## Nommage

Pour vous référer à des conventions et un coding style, je vous invite à observer le PSR-1 et les ressources Symfony.

[PSR-1](https://www.php-fig.org/psr/psr-1/)

[Coding Standards](https://symfony.com/doc/current/contributing/code/standards.html#structure)

[Convention](https://symfony.com/doc/current/contributing/code/conventions.html)

## CLI

Avec ce Framework nous disposons d'un utilitaire pour automatiser des taches et nous offrir des fonctionnalité: la console.

```
php bin/console
```

Nous observons majoritairement des commandes concernant:

- Le cache
- Le debug
- L'accès aux données
- La création

----------

[Retour au sommaire](00_sommaire.md)
