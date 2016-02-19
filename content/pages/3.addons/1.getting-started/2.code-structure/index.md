---
title: Code Structure
id: 4fe58b57-e9ae-4417-8314-0e48bcb18e60
overview: |
  Learn about all the moving pieces of an addon - how they should be named and where they belong.
---
Your addon won't do anything without some code.

Statamic uses PSR-4 Autoloading to load addons in the `site/addons/` folder. This basically means that as long as you name your files correctly, your addons will start working just by existing.

Your addon’s folder needs to live in a directory named after the StudlyCased name of your addon. Each addon aspect should be named `[AddonName][Aspect].php`.

Here's an example of a fully loaded addon.

``` .language-files
site/
└── addons/
    └── Bison/
        |-- Commands/
        |   |-- ClearOrdersCommand.php
        |   └── CreateProductCommand.php
        |-- Models/
        |   |-- Order.php
        |   └── Product.php
        |-- BisonAPI.php
        |-- BisonController.php
        |-- BisonFieldtype.php
        |-- BisonListener.php
        |-- BisonModifier.php
        |-- BisonServiceProvider.php
        |-- BisonTags.php
        |-- BisonTasks.php
        |-- bootstrap.php
        |-- composer.json
        └── meta.yaml
```

Note: The `Models` directory is not Statamic-specific. It is just a way you might organize your code. See [Keeping DRY][dry].


[dry]: /addons/best-practices/keeping-dry
