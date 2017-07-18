---
id: 8aa482b3-cc4c-4099-81c6-cac84bcc6c76
title: Getting Started
---
## Eager to dive in? {#please}

Welcome, fellow developer. We'll give you a tip straight out of the box. Become friendly with the `please` console command. You can do a bunch of neat things from there. One of which is to create a new addon:

``` .language-bash
php please make:addon MyAddon
```

It'll ask you some questions and generate all the necessary boilerplate files you'll need. Whether you need just a Tags class or the whole kaboodle, this command will be your friend.

## Meta Data {#meta}

The first step in building an addon is to create a metadata file. This tells Statamic about your addon and will display it in the addon manager.

Simply create a `meta.yaml` file in the root of your addon folder, and swap out our fictional addon details below with your own.

``` .language-yaml
name: Charging Bison
version: 1.0
description: Charge your customers with the grace of a Bison.
addon_url: http://statamic.com/addons/charging-bison
developer: Statamic
developer_url: http://statamic.com
commercial: true
```

Congratulations, you've built an addon!

Well, kinda. But it does nothing. That's like saying breakfast is ready but all you've done is set the table.
Now you'll need to cook the bacon – er, [write some code](#code-structure).

**Note:** Technically this step is only required if you plan to display details in the _Addons_ section of the control panel. If you want to whip up something quick for yourself, you don't need a meta file. You may even consider creating a [site helper](/addons/site-helpers) instead of an addon.

Marking your addon as `commercial` won't dump money into your bank account, but it will automatically add a `license_key` field to your [settings page](/addons/classes/settings#ui).


## Code Structure {#code-structure}

Your addon won't do anything without some code.

Statamic uses PSR-4 Autoloading to load addons in the `site/addons/` folder. This basically means that as long as you name your PHP files correctly, your addons will start working just by existing.

Your addon’s folder needs to live in a directory named after the StudlyCased name of your addon.  
Have an addon named `Bacon Bits`? You'll want to create `site/addons/BaconBits/`.

**Note: Case is important!** Be sure to name your files and class names in **StudlyCase**.

OSX/macOS is case _insensitive_. Linux is case _sensitive_.

A common problem developers run into is while developing locally everything seems fine, but after pushing to production, things don't work.

What usually happens here is that when Statamic requests `SomeClass.php`, your Mac will happily provide `someclass.php`. But once you push to a server, typically running Linux, it'll only look for `SomeClass.php`, and _not_ `someclass.php`.

## Installing an Addon {#installing}

Most of the time, an addon is considered "installed" just by dropping the files into your project.

However, if your addon has any [Composer dependencies](/addons/bootstrapping#composer), it won't be considered installed until you've brought those into Statamic. You can do that with this command:

``` .lang-bash
php please update:addons
```

If you want to programatically know if an addon is installed, you can do this by `$addon->isInstalled()`.
