# Découverte de Symfony

## [Profiler](https://symfony.com/doc/current/profiler.html)

Le profiler se composer d'une barre de debug ouvrant le Web Profiler.

### 🏷️ **Debug Tool Bar**

Se trouvant en bas de la fenêtre cette barre donnes un récapitulatif de la requête.

![image](ressources/debug_bar.png)

> Attention elle n'est visible que quand le document est complet et bien formé. Dans le cas ou un controller ne forme pas un document HTML elle ne sera pas visible.

En cliquant sur une information elle ouvre le Web Profiler.

### 🏷️ **Web Profiler**

Il contient des informations exhaustive sur la requête par section.

![image](ressources/web_profiler.png)

Vous trouvez des informations sur la requête, la validation des formulaires, la traduction, les exceptions, les logs, l'accès aux données, la sécurité, le système de templates et toute fonctionnalité que la requête a pu activer.

----------

## 📑 [Logs](https://symfony.com/doc/current/logging.html)

Chaque erreur est logée dans un fichier.

Par défaut le log se fait en mode développement et production, mais en production seul les error, critical, alert et emergency seront logés...

Il est possible de spécifier différents fichiers de log par [channel](https://symfony.com/doc/current/logging/channels_handlers.html) (doctrine, event, security, request).

----------

## 📑 [Configuration](https://symfony.com/doc/current/configuration.html)

Chaque fonctionnalité peut se configurer dans le dossier `config/`. Quand il s’agit de fonctionnalité additionnelles, leur configuration se fait dans `config/packages`.

Par exemple pour configurer un fichier de log spécifique pour les erreurs http, il faudra en fonction de l'environnement modifier le fichier de configuration correspondant.

-   config/packages/dev/monolog.yaml

```yaml
monolog:
    handlers:
        http:
            type: stream
            path: "%kernel.logs_dir%/%kernel.environment%.request.log"
            level: debug
            channels: ["request"]
        main:
            type: stream
            path: "%kernel.logs_dir%/%kernel.environment%.log"
            level: debug
            channels: ["!event", "!request"]
```

Chaque configuration doit se modifier en étudiant la documentation en rapport.

----------

[Retour au sommaire](00_sommaire.md)
