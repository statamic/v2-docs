---
id: 8aa482b3-cc4c-4099-81c6-cac84bcc6c76
title: Getting Started
---
## Eager to dive in?

Welcome, fellow developer. We'll give you a tip straight out of the box. Become friendly with the `please` console command. You can do a bunch of neat things from there. One of which is to create a new addon:

``` .language-bash
php please make:addon MyAddon
```

It'll ask you some questions and generate all the necessary boilerplate files you'll need. Whether you need just a Tags class or the whole kaboodle, this command will be your friend.

## Meta Data

The first step in building an addon is to create a metadata file. This tells Statamic about your addon and will display it in the addon manager.

Simply create a `meta.yaml` file in the root of your addon folder, and swap out our fictional addon details below with your own.

``` .language-yaml
name: Charging Bison
version: 1.0
description: Charge your customers with the grace of a Bison.
addon_url: http://statamic.com/addons/charging-bison
developer: Statamic
developer_url: http://statamic.com
```

Congratulations, you've built an addon!

Well, kinda. But it does nothing. That's like saying breakfast is ready but all you've done is set the table.
Now you'll need to cook the bacon – er, [write some code][code-structure].

**Note:** Technically this step is only required if you plan to display details in the _Addons_ section of the control panel. If you want to whip up something quick for yourself, you don't need a meta file.


## Code Structure

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
        |-- resources/
        |   |-- assets/
        |   |   |-- js/
        |   |   |-- css/
        |   |   `-- img/
        |   |-- lang/
        |   |   `-- en/
        |   |       `-- settings.php
        |   `-- views/
        |       `-- index.blade.php
        |-- BisonAPI.php
        |-- BisonController.php
        |-- BisonFieldtype.php
        |-- BisonListener.php
        |-- BisonModifier.php
        |-- BisonServiceProvider.php
        |-- BisonTags.php
        |-- BisonTasks.php
        |-- BisonWidget.php
        |-- bootstrap.php
        |-- composer.json
        |-- default.yaml
        |-- routes.yaml
        |-- settings.yaml
        └── meta.yaml
```

**Note:** The `Models` directory is not Statamic-specific. It is just a way you might organize your code. Totally up to you.

[dry]: /addons/best-practices/keeping-dry
