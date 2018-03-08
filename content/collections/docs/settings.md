---
title: Settings
id: 00b392fc-1fdc-4699-8e73-438da24fc4fc
overview: >
  Statamic has many configuration options that allow for a lot of flexibility in behavior and functionality. Settings can be set in the Control Panel, your YAML files, and even your `.env` file if you would like them to differ depending on your environment.
---
## Overview

The settings are grouped into categories to help you find related options quickly. All of Statamic's settings are available in the Control Panel's `Configure » Settings` area.

![Settings](/assets/img/screenshots/cp-settings.jpg) {.rounded}

On the filesystem side, Statamic stores its settings into groups located in `site/settings/[group_name].yaml`.

The files are organized based on their general functions. For example, `assets.yaml` configures assets and image manipulation, `caching.yaml` for cache settings, `system.yaml` for general system settings, and so on.


## Addons {#addon}

Each addon can have their own settings file located in `site/settings/addons/addon_name.yaml`. Addon settings can be overridden in your environment files. See below for more details.


## Environment Specific {#environment}

Sometimes it is beneficial to have different settings depending on where you are running the site. For instance, enabling debug mode when in development, but not in production. Or perhaps running some analytics javascript snippet only on production.

You can do this by overriding settings based on the environment you are serving the site.

[Read about environments](/environments)


## Maintenance Mode {#maintenance-mode}

When your site is in maintenance mode, a _Be Right Back_ message will be displayed for all requests. This could be
useful to "disable" your site while you perform updates.

To enable it run the `down` command, and to disable it run the `up` command:

``` .language-bash
php please down   # enable maintenance mode. take your site "down"
php please up     # disable. bring your site back "up"
```

This feature is the same as [Laravel's](https://laravel.com/docs/5.1/installation#maintenance-mode). You may customize the view by adding a `503.html` error template to your theme.
