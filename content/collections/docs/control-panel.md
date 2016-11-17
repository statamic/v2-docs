---
title: Control Panel
id: 34addbbd-c904-4a02-ad54-bf49cc95c6d7
overview: The Statamic Control Panel enables you to publish content, manage users, configure settings, run updates, and all manner of other useful things. And it's responsive.

---
## Logging In {#logging-in}

To login to Statamic one must simply visit `example.com/cp`.

![Control Panel](/assets/img/screenshots/cp-login.jpg)

## Disabling the Control Panel {#disabling}

If you'd like to manage your site with files only, you can disable the Control Panel entirely by setting `$control_panel = false;` in your `index.php` file.

## Translating the Control Panel {#translating}

By default, the Statamic Control Panel will be displayed in your default locale. Check out the [localization](/localization) page for more details.
Your default locale is what you've set in `index.php` and the first locale listed in the `locales` array in `system.yaml`.

You may override the Control Panel's locale by setting the `locale` value in either `site/settings/cp.yaml` to apply to all users, or in a user's file
to apply to only that user.

For example, to display the Control Panel in French, you might add this:

``` .language-yaml
locale: fr
```

Statamic comes bundled with translation files for English. These files are located in `statamic/resources/lang/en`.
If you would like to translate into another language, you may copy the `en` folder into `site/lang` and rename `en`
to the locale of your choice, then edit the strings within them.

We will be compiling a list of community provided translation files soon.