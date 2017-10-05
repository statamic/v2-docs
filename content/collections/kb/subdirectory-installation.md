---
title: Installing in a Subdirectory
kb_categories:
  - Tips, Tricks, and How-Tos
id: ca8c33b2-f64b-4cb3-b436-8cd55b33dc03
---
Let's pause for just a moment. Do you want to run in a subdirectory for the right reason? Using Statamic in a `blog` subdirectory in an existing site is one such reason. Not feeling like setting up a virtual host isn't. We can't stop you, but if you plan to run the site in webroot in production, you should do the same thing in development.

## Installation {#installation}

Professional advice given, here's what to do:

- Open `index.php` and change `$site_root` from `"/"` to `"/your_subdirectory/"`
- Open `site/settings/system.yaml` and change the URL from `/` to `/your_subdirectory/`


## Links {#links}

We recommend any hardcoded links in your templates to be wrapped in the [`path` tag](/tags/path) (or the `link` tag, they are the same thing).

This tag will automatically prepend your site root to the URL.

If you're running in a subdirectory, it'll prepend it. If you ever decide to run without a subdirectoy, it wont, so you won't need to update your templates.

``` .language-yaml
---
# inside system.yaml
url: /subdir/
```

```
<a href="{{ link to="/about" }}">
```

``` .language-output
<a href="/subdir/about">
```

## Assets {#assets}

When using the Control Panel to select assets in your content, they will be saved to your YAML files _without_ the subdirectory.
In other words, their URLs will be _relative to your Statamic site_.

It does this so that if you ever decide to _not_ run in a subdirectory, or you need to rename it, you won't need to update 
all the asset URLs throughout your content.

For example, if your asset container is located at `yoursite.com/subdirectory/assets` then assets would be saved to YAML
like this:

``` .language-yaml
---
images:
  - /assets/one.jpg
  - /assets/two.jpg
```

Now, simply looping through and outputting these values inside img tags will result in 404s because the subdirectories are missing.

To resolve this, you may either use the path/link tag as mentioned above; or use a combination of the [Assets](/tags/assets) and the
`permalink` variable.

```
{{ images }}
    {{ link :to="value" }}
{{ /images }}

{{ assets:images }}
    {{ link :to="url" }} or {{ permalink }}
{{ /assets:images }}
```

``` .language-output
/subdir/assets/one.jpg
/subdir/assets/two.jpg

/subdir/assets/one.jpg or http://yoursite.com/subdir/assets/one.jpg
/subdir/assets/two.jpg or http://yoursite.com/subdir/assets/two.jpg
```
