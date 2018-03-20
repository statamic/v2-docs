---
title: Installing and running above webroot
id: 3ae9a8ef-c2e4-40c5-a9bd-c1203bab9203
overview: Placing your system files out of reach adds another layer of security.
kb_categories:
  - Tips, Tricks, and How-Tos
---
Statamic comes bundled with everything in the root folder, intended to be placed directly in your webroot. This makes it easier to just drop your site into a server and be on your way.

While things like `.htaccess` and `nginx.conf` files can ensure important files aren't accessible, it's always possible
that those things are forgotten or misconfigured. Taking the extra step to place the folders out of reach entirely
will give you that additional peace of mind.

**tl;dr** [Here's a summary of the steps](#summary).

## Move the files

Out of the box, you'll see something like this: (some things left out for brevity)

``` .language-files
assets/
local/
site/
statamic/
index.php
please
.htaccess
.gitignore
```

The webroot will be wherever `index.php` is.
You should ensure that the folders are located one level above, like this:

``` .language-files
local/
public/
|-- assets/
|-- index.php
|-- .htaccess
site/
statamic/
please
.gitignore
```

In our example, the `public` folder will be the webroot.

### Web accessible files

The default Statamic folder structure can safely assume everything will be accessible. Now its not the case, so some things
will need to be adjusted.

Your `themes` folder and any public assets will likely need to be web-accessible if you plan on accessing them.
That's logical, right? We recommend moving them into a location like the following, but of course you may move
them wherever you want:

``` .language-files
public/
|-- assets/
|-- themes/
|   |-- your-theme/
`-- index.php
```


## Let Statamic know where everything went

### The statamic folder

The `index.php` file needs to know where the `statamic` folder is located. Now that it has been moved, you'll need to
update it.

Open the `index.php` file, and update the `$statamic` variable.

By default it is `./statamic` (meaning the same directory level), and in our example, we'd update it to `../statamic`
(meaning _up_ one level).

### Everything else

Now that the folders have been moved, you'll need to tell Statamic where they are.

We've organized Statamic into different filesystems to make file management a breeze. We've written [a whole article about it][filesystems], if you're interested in learning more.

Within that article, it explains how to adjust the locations of your folders. In short, you'll want to adjust the
`root` and `url` values for each `filesystem` in `site/settings/system.yaml` to correspond with their new locations.

You'll also want to update your asset container `path` and `url` values, which can be done in `site/storage/[container-id]/container.yaml`.

## Summary {#summary}

1. Move system folders above webroot.
2. Ensure web accessible files are left in the webroot. (`assets`, `site/themes`, `index.php`, etc.)
3. Adjust the `$statamic` path variable in `index.php`.
4. Adjust [filesystem references][filesystems].
5. Adjust the asset container's path in `container.yaml`.

[filesystems]: /knowledge-base/filesystems
