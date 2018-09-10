---
title: Localization
id: 30959b92-e1a5-4abf-aca7-c8f2e97050f7
overview: >
  By configuring additional locales, content becomes translatable with the flick of a switch. Each language can run on its own URL, so you can have `example.com` and `example.com/de` or `fr.example.com`, or `example.de`. Lots of control. Lots of options.
---

## URL Structure {#url-structure}

The first step in localizing a Statamic site is to decide on the URL structure.

A common convention is to create subfolders for each non-default locale.

For example:

- `http://example.com/` for English
- `http://example.com/fr/` for French
- `http://example.com/de/` for German
- and so on...

In this guide, we will assume we're using this subfolder approach in any examples. If you decide to go a different method, make sure any relative paths are updated for your situation. [See below for the subdomain version](#subdomains).

## Creating the locale roots {#locale-roots}

Following from our subfolder example, we'll need to create those folders.

For each locale, you should copy the `index.php` (and `.htaccess` if using Apache) into their respective folders.

``` .language-files
/ /
|-- assets/
|-- local/
|-- site/
|-- statamic/
|-- fr/
|   |-- index.php
|   `-- .htaccess
|-- de/
|   |-- index.php
|   `-- .htaccess
|-- index.php
|-- .htaccess
|-- ...etc...
```

In the new `index.php` files, you should adjust a few variables:

The relative path to the `statamic` folder needs to be updated to reflect the new location.

``` .language-php
$statamic = '../statamic';
```

The site root should also be updated. Since we're running in a subfolder, you should set this appropriately. (Take note of the trailing and leading slashes.)

``` .language-php
$site_root = '/fr/';
```

Lastly, the locale. This should correspond with the locale you will be adding to the system settings in the next step.

``` .language-php
$locale = 'fr';
```

### Nginx {#nginx}

If you're using Nginx, you'll want to slap one of these bad boys in for each locale.

``` .language-nginx
location @frrewrites {
    rewrite ^/fr/(.*)$ /fr/index.php last;
}

location /fr/ {
    try_files $uri $uri/ @frrewrites;
}
```

Laravel Valet users shouldn't need to do anything to their nginx config. If you are using a non-standard subdirectory, see the [troubleshooting section](#laravel-valet) below.

## Adding Locales to Settings {#locales}

Next, Statamic will need to know we intend to localize our content.

Head to `System > Settings > System` and add your locales to the `Locales` field. In this field you will need to provide:

- The shorthand locale string. – This is what you added to `$locale` in `index.php`. (eg. `fr`)
- The full locale. – This is what PHP uses to format date and other strings. (eg. `fr_FR`)
- Name – This is what will be used throughout the Control Panel when referring to the locale. (eg. `French`)
- URL – This is the URL of the homepage for that locale. (eg. `http://example.com/fr/`)

If you're not using the CP, you can do the above by adding an array to `site/settings/system.yaml`, like so:

``` .language-yaml
locales:
  en:
    full: en_US
    name: English
    url: http://example.com/
  fr:
    full: fr_FR
    name: French
    url: http://example.com/fr/
```

> If you are changing the default locale away from `en`, you should also change `$locale` in your `please` file.

## Localizing your content {#localizing-content}

### Fields {#fields}
Once you've added your locales, you need to define which fields may be translated.

You can do this by toggling the "Localizable" option for each field you wish to translate. If you aren't using the CP, you can just add `localizable: true` to each field in your fieldset.

### Editing {#editing}
Now, when _editing_ content, you should see a "Locales" list in the sidebar. This shows you all of your available locales. A checkmark indicates the locale you are currently editing, a green dot indicates that locale is available (published), and a white dot means the content is unavailable (draft).

Selecting one of those locales will take you to edit the same page, but only fields marked as localizable will be available.

### Files {#files}

For those of you that are not using the control panel, or if you are just interested, here's how localizing works with files.

``` .language-files
site/content/
|-- pages/
|   |-- biography/
|   |   |-- index.md
|   |   |-- fr.index.md
|   |   `-- de.index.md
|   `-- index.md
|-- collections/
|   `-- blog/
|       |-- fr/
|       |   `-- 1.my-post.md
|       |-- de/
|       |   `-- 1.my-post.md
|       `-- 1.my-post.md
`-- taxonomies/
    `-- categories/
        |-- fr/
        |   `-- news.md
        |-- de/
        |   `-- news.md
        `-- news.md
```

In Pages, each folder represents a page. The default locale is simply named `index.md`. Any additional locales are named with their locale prefix. eg `fr.index.md`.

In both Collections and Taxonomies, the localized entries/terms are all stored in subfolders with identical filenames to the default locale. eg. `categories/news.md` and `categories/fr/news.md`.

For pages, entries, and taxonomy terms: slugs may be localized by adding a `slug` field to the front-matter. The filenames shouldn't change. They should also all have `id` fields that match their default counterparts.

### Publish States / Statuses {#publish-states}

It's possible to have a different status per locale. For instance, you may want to have a page that only exists in one locale (or multiple, but not all of them).

In the Control Panel this should be straightforward: There is a toggle next to the locale you're editing. Toggle it off to set it as a draft.

In the files, the default locale's status is defined in the filename as per usual (an underscore to make it a draft). For additional locales, you may add a `published: true` or `false` to their front matter.

Keep in mind that additional locales will inherit the status of the default locale. If you change the status of the default locale, the other locales will also become drafts unless you already added a `published` value to their front-matter.

### Routing {#routing}

It's possible to assign a different collection and/or taxonomy routes for each locale. For example, you may want `/news/my-post` for English, but `/noticias/mi-publicacion` in Spanish.

Instead of a string for the route, you can simply specify an array with each locale, like so:

``` .language-yaml
collections:
  news:
    en: /news/{slug}
    es: /noticias/{slug}
```


## Displaying your content {#diplaying}

When browsing your site, the locale of content that will be displayed will be determined by the URLs defined in your system settings.

For example, if we visit `/about`, it would load the default locale (English, in this example), and if we visit `/fr/about`, it would load the French locale.

If you were to output a field that isn't localizable, or a field that just hasn't been localized, it would display the default value.

Take this "biography" page (`site/content/pages/biography/index.md`), for instance:

``` .language-yaml
---
id: 123
title: Biography
portrait: me.jpg
color: red
---
I love bacon.
```

And here's the French equivalent (`site/content/pages/biography/fr.index.md`):

``` .language-yaml
---
id: 123
title: Biographie
slug: biographie
---
J'adore le bacon
```

Lastly, take this template:

```
<h1 style="color: {{ color }}">{{ title }}</h1>
<img src="{{ portrait }}" />
{{ content }}
```

When visiting `/biography`, you'd see:

```
<h1 style="color: red">Biography</h1>
<img src="me.jpg" />
I love bacon
```

And when visiting `/fr/biographie`, you'd see:

```
<h1 style="color: red">Biographie</h1>
<img src="me.jpg" />
J'adore le bacon
```

There are 3 things to notice:

1. The URL for the French site would use the localized slug
2. We'll assume the `image` field didn't have `localized: true` in the fieldset.   That means it wouldn't have appeared in the publish form for the French locale. Also, since it's not defined in the French file, it falls back to the default locale's value.
3. Assuming `{{ color }}` simply was left blank, or it had the same value as the default locale, it will fall back to the default locale's value.

## Subdomains {#subdomains}

The examples in this guide are for a typical subdirectory-based installation. As mentioned above, the steps outlined will
be the same for any setup, but with any paths adjusted for your needs. Another common solution is to use subdomains.
For example, `mysite.com`, `fr.mysite.com`, etc.

Here's a brief rundown of how to setup subdomain locales, assuming you have the same folder structure as mentioned above.

- Instead of simply visiting `mysite.com/fr/`, you'd point your subdomain to the folder.
- The `$statamic` path wouldn't need to change in this example, but if your `fr` folder is located elsewhere it will need to.
- The `$site_root` should be `"/"` since there's no subdirectory in the URL.
- Links to your theme assets won't work because they will be relative, and the files don't actually exist there.  
  You can symlink `site/themes/` to inside your `fr` folder. That'll do the trick.
- You can consider symlinking your public asset container folders in the same fashion.

## Translating the Control Panel {#control-panel}

You are able to translate the control panel. This is a separate process to localizing your site. The CP may be translated
into languages other than what your site uses.

Read about it on the [Control Panel](/control-panel#translating) page.

## Troubleshooting {#troubleshooting .mb-3}

### I added a localized file but I get an error telling me the ID already exists.

This will happen if you haven't added your additional locale to your `system.yaml` file.

### My subdirectory shows a 404 when using Laravel Valet. {#laravel-valet}

The Statamic driver included with Valet [supports ISO3166 alpha-2 codes as subdirectories](https://github.com/laravel/valet/blob/master/cli/drivers/StatamicValetDriver.php#L116-L129).
If you need something not supported, you will have to create a custom local driver.

Create a `LocalValetDriver.php` in your project root with the following:

``` .language-php
<?php
class LocalValetDriver extends StatamicValetDriver
{
    public function getLocales()
    {
        return ['one', 'two', 'etc']; // ...where these are the subdirectories you need.
    }
}
```
