---
title: Settings
id: 00b392fc-1fdc-4699-8e73-438da24fc4fc
---
## General settings

Statamic stores its settings into groups located in `site/setttings/[group].yaml`. These various files map to pages in
the CP under `System > Settings`.

The settings files are organized based on their general functions. For example, `assets.yaml` configures assets and image manipulation, `caching.yaml` for cache settings, `system.yaml` for general system settings, and so on.


## Addon settings

Each addon can have their own settings file located in `site/settings/addons/addon_name.yaml`.

Addon settings can be overridden in your environment files. See below for more details.


## Environment Specific Settings

Sometimes is beneficial to have different settings depending on where you are running the site. For instance, enabling debug mode when in development, but not in production. Or perhaps running some analytics javascript snippet only on production.

You can do this by overriding settings based on the environment you are serving the site.

[Read about environments](/environments)