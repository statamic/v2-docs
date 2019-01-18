---
title: Bootstrapping
id: 0398c16a-8861-4efb-8a68-dbea47c5f446
---

## Third Party Dependencies (Composer) {#composer}

Statamic will handle the installation of any Composer dependencies you may need.

Drop a `composer.json` in your addon's directory. Statamic will merge your dependencies in with its own when updating through the Control Panel,
or when running the update command.

``` .language-javascript
{
  "name": "bob/html-to-markdown",
  "require": {
    "league/html-to-markdown": "~4.1"
  }
}
```

- The `name` key should just be in the format of `developer-name/addon-name`.
- Your addon does **not** need to be registered on Packagist. 
- There's no need to bundle a `vendor` folder with your addon.

To install the dependencies, run the following command:

```
php please update:addons
```

## Dependency Injection {#depdency-injection}

Addon classes will be constructed through [Laravel's service container][laravel-container]. In a nutshell, this means that you may typehint any classes in your constructor, and they will be passed in automatically.

``` .language-php
public function __construct(Bacon $bacon)
{
    //
}
```

**Note:** When using the constructor like this, you will lose the automatic [bootstrapping](#bootstrapping) and [initializing](#initializing) features. You can use `$this->bootstrap();` in your constructor to bring it back. As for initializing, just use the constructor.

## Bootstrap File {#bootstrap}

**This feature is deprecated since 2.6**  

Addon classes will automatically require a `bootstrap.php` file in your addon's root directory.

However, you should consider using [Composer](#composer) and/or PSR-4 autoloading instead.

## Initializing {#initializing}

**This feature is deprecated since 2.6**  

Prior to Statamic 2.6, addons were loaded in a way that prohibited the use of constructors in your own classes. Adding an `init` method was akin to a constructor.
This method was called within the constructor.

Now that you have control over your own constructors, you may add your initialization in there.

[laravel-container]: https://laravel.com/docs/5.1/container
