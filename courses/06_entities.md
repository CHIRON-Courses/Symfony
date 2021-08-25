# Les entités

## [Database](https://symfony.com/doc/current/doctrine.html#configuring-the-database)

Vos information de base de données doivent être spécifiés dans le fichier `.env`.

```
DATABASE_URL="mysql://db_user:db_password@127.0.0.1:3306/db_name?serverVersion=5.7"
```
```
DATABASE_URL="mysql://root@127.0.0.1:3306/symfony_decouverte?serverVersion=5.7"
```

Une fois ces information spécifiés vous pouvez créer votre base avec la commande suivante.

```
bin/console doctrine:database:create
```

N'hésitez pas à rattacher votre IDE à votre database.

----------

## [Make](https://symfony.com/doc/current/doctrine.html#creating-an-entity-class)

Un utilitaire nous propose de créer des entities.

```
bin/console make:entity
```

L'utilitaire demande un nom d'entité puis propose de créer ses attributs. Une entité deviendra une table. Chaque entité possède un `id` qui est géré par l'ORM doctrine et qui ne doit pas être spécifié.

L'utilitaire ne propose pas de rajouter la contrainte unique.

> Il est possible de générer les entities à partir d'une base de données.

[Reverse Engineering](https://symfony.com/doc/current/doctrine/reverse_engineering.html)

### Unique

Pour ajouter la contrainte sur une colonne il faut la préciser dans l'annotation en rapport.

```php
/**
 * @ORM\Column(type="string", length=255, unique=true)
 */
```

Pour ajouter une contrainte sur plusieurs colonnes il faut le spécifier sur l'annotation de la table.

[Unique Entity](https://symfony.com/doc/current/reference/constraints/UniqueEntity.html)

CF:

```php
/**
 * @ORM\Entity
 * @ORM\Table(name="foo",uniqueConstraints={@ORM\UniqueConstraint(name="name_email_u", columns={"name", "email"})})
 */
```

----------

## [Migration](https://symfony.com/doc/current/doctrine.html#migrations-creating-the-database-tables-schema)

Nous pouvons à partir de vos entities créer les tables. C'est une manipulation en deux étapes.

-   Créer le schéma de migration

```
bin/console make:migration
```

-   Exécuter la migration

```
bin/console doctrine:migrations:migrate
```

Quand vous modifiez une entity, il faut refaire la migration. Cependant doctrine possède un outil pour forcer la mise à jour des tables en cas de conflit de migration

-   Update des tables sans migration

```
 bin/console doctrine:schema:update --force --dump-sql
```

----------

## [Relations](https://symfony.com/doc/current/doctrine/associations.html)

Une entité peut être en relation avec d'autres par les relations `OneToOne`, `OneToMany` et `ManyToMany`.

Au moment de choisir le type de la colonne, il est possible de demander "`?`" pour avoir un tutoriel sur les relations et nous propose de réfléchir pour proposer une de ces 3 relations.

----------

[Retour au sommaire](00_sommaire.md)
