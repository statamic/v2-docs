---
title: Addon Structure
overview: >
  You Addon can contain one or many different classes that serve various purposes. Each will inherit functionality, and therefore will `Extend` the core `Statamic\API\Addon` class, either directly, or abstractly. 
id: 395dc432-0273-4874-873e-6ed0b772cc27
---
## The Addon Class {#addon}

This core class provides will automatically provide your Addon with contextual information, settings, context, and helper methods, all as part of `$this->`.

## Helpers {#helpers}

Whether you're retrieving a tag parameter, a config value, or manipulating the cache; there are various helper methods available throughout an addon package. 

There is a whole section dedicated to the [helper methods][helpers] available throughout your addons.

## Bootstrapping {#bootstrapping}

An addon will automatically look for a `bootstrap.php` file in your directory. You can use this file to add any configuration you might need available in every aspect of your addon.

For example, if you need to load a custom library, you might want to do that here.

```.language-php
<?php

require_once __DIR__.'/libs/library.php';
```

## Dependencies {#dependencies}

Statamic will handle the installation of any Composer dependencies you may have. Simply drop a `composer.json` in your addon's directory.

``` .language-javascript
{
  "name": "bob/html-to-markdown",
  "require": {
    "league/html-to-markdown": "~4.1"
  }
}
```

Your addon _does not_ need to be registered on Packagist. The `name` key should just be in the format of `developer-name/addon-name`.

There's no need to bundle a vendor folder with your addon.

## Initializing {#initializing}

The nature of how Statamic loads some of your classes means that a constructor method isn’t really suitable for initializing any values you might need available to other methods. You can create an init() method which acts as a pseudo constructor. This gets called during the parent constructor.

``` .language-php
<?php namespace Statamic\Addons\MyAddon;

class MyAddonTags extends \Statamic\Extend\Tags
{
    private $thing;

    public function init() {
        $this->thing = new Thing;
    }

    public function myTag() {
        return $this->thing->doThing();
    }
}
```

An alternative to the `init()` method is to use a [Service Provider][service-provider] to bind classes into Laravel’s Service Container.

[helpers]: /addons/helpers
[service-provider]: /addons/structure/service-providers
