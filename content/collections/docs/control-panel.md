---
title: Control Panel
id: 34addbbd-c904-4a02-ad54-bf49cc95c6d7
overview: The Statamic Control Panel enables you to publish content, manage users, configure settings, run updates, and all manner of other useful things. It's responsive, intuitive, and powerful.

---
## Logging In {#logging-in}

You can access the Statamic Control Panel by visiting `yourwebsite.com/cp`. If you'd like to use a different URL, you can change the `$control_panel` variable in your `index.php` file accordingly.

```language-php
$control_panel = 'chamber-of-secrets';
```


![Control Panel](/assets/img/screenshots/cp-login.jpg)

## Disabling {#disabling}

Disabling the control panel is done in the `index.php` file as well.

```language-php
$control_panel = false; // disabled
```

## Translating {#translating}

By default, the Statamic Control Panel will be displayed in your default locale. That locale is the first one listed in your `locals` list in your System settings, which should also match the `$locale` setting in your `index.php` file.

```language-yaml
# system.yaml
locales:
  en:
    name: English
    full: en_US
    url: /
```

```language-php
// index.php
$locale = 'en';
```

The translation files for English are included, and are located in `statamic/resources/lang/en`. If you would like to translate another language, copy the `en` folder into `site/lang` and rename it to the locale of your choice, and begin editing the data inside.

You may override the Control Panel's locale by setting the `locale` value in either `site/settings/cp.yaml` to apply to all users, or in a user's file
to apply to only that user.

For example, to display the Control Panel in French you would add the following:

``` .language-yaml
locale: fr
```

Learn more about [localization](/localization).