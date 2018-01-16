---
id: 8aa482b3-cc4c-4099-81c6-cac84bcc6c76
title: Getting Started
---
## Eager to dive in? {#please}

Welcome, fellow developer! We'll give you a tip straight out of the box. Become friendly with the `please` console command. You can do a bunch of neat things from there. One of which is to create a new addon:

``` .language-bash
php please make:addon MyAddon
```

It'll ask you some questions and generate all the necessary boilerplate files you'll need. Whether you need just a `Tags` class or the whole kaboodle, this command will be your friend.

## Meta Data {#meta}

The first step in building an addon is to create a metadata file. This tells Statamic about your addon and will display it in the addon manager.

Create a `meta.yaml` file in the root of your addon folder, and swap out our fictional addon details below with your own.

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

Well, kinda. It doesn't do anything yet, but not to worry. We'll get there.

> Technically this step is only required if you plan to display details in the _Addons_ section of the control panel.

If you want to whip up a little doo-dad and don't plan to share it with the world, you probably don't need a meta file. You may even consider creating a [site helper](/addons/site-helpers) instead of an addon.

Marking your addon as `commercial` won't dump money into your bank account (sorry, we wish it did!), but it _will_ automatically add a `license_key` field to your [settings page](/addons/classes/settings#ui).


## Code Structure {#code-structure}

Statamic uses PSR-4 Autoloading to load addons in the `site/addons/` folder. This basically means that as long as you name your PHP files correctly, your addons will start working just by existing.

Your addon’s folder needs to live in a directory named after the StudlyCased name of your addon. Have an addon named `Bacon Bits`? You'll want to create `site/addons/BaconBits/`.

> Case is important! Be sure to name your files _and_ class names in **StudlyCase**.

OSX/macOS is case **insensitive**. Linux is case **sensitive**. This mismatch in behavior can lead to things working locally in development and then suddenly **not** working when your Linux production server interprets filenames differently.

What often can happen is when Statamic requests `SomeClass.php`, a Mac will try to be nice and provide the closest alternative, such as `someclass.php`, but once you push to a server running Linux, it will be looking for `SomeClass.php`, and things break. Oops.

## Installing an Addon {#installing}

Most addons are considered "installed" simply by the presence of their files being in the proper location of your site.

However, if your addon has [Composer dependencies](/addons/bootstrapping#composer) it won't be treated as "installed" until the depedencies have been installed too. You can do this by running the following command command:

``` .lang-bash
php please update:addons
```

If you want to determine if an addon is installed programmatically, you can do this with the function `$addon->isInstalled()`.
