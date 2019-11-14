---
title: Installing Statamic
overview: Statamic is one of the simplest web applications to get up and running, but there are many different ways to run PHP apps, so we'll cover a lot of variations here. Feel free to skip around until you find your particular dev environment. It's unlikely that _everything_ in this article will apply to your personal style.
id: a7668389-57ab-47aa-b26f-6d2299b5b534
---

## The Screencast Version {#screencast}

Are you a visual learner? Watch how to install Statamic.

<div class="video-embed">
<iframe src="https://player.vimeo.com/video/165632057?title=0&byline=0&portrait=0" width="800" height="450" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
</div>


## Requirements {#requirements}

Statamic has a few server requirements. Not all servers are created equal and not all hosts play nice. Statamic v2 is built on [Laravel][laravel], so as a rule of thumb -- anywhere Laravel 5.1 runs, Statamic runs. You can even use [Forge][forge] + [Digital Ocean][do]. We do, and we love it. Anyway, here's what you're going to need.

### Server Requirements {#server-requirements}

- A web server: Apache, Nginx, or IIS
- PHP >= 5.5.9
- URL Rewriting enabled (mod_rewrite, try_files, etc)

### Required PHP Extensions {#required-php-extensions}

These should all be included in PHP 5.5.9+ by default, but in case you're a tinkerer, you're going to need these:

- [OpenSSL](http://php.net/manual/en/book.openssl.php)
- [Multibyte String](http://php.net/manual/en/book.mbstring.php)
- [Tokenizer](https://secure.php.net/manual/en/book.tokenizer.php)
- [JSON](https://secure.php.net/manual/en/book.json.php)
- [PCRE](http://php.net/manual/en/book.pcre.php)
- [GD](http://php.net/manual/en/book.image.php) or [ImageMagick](http://php.net/manual/en/book.imagick.php)
- [FileInfo](http://php.net/manual/en/book.fileinfo.php)

### Optional (But Recommended) {#optional-requirements}

- [Composer](https://getcomposer.org/)


## Suggested Development Environments {#dev-environments}

You can (and probably should) run Statamic **locally** while you develop your site. There are a number of solutions that give you all the tools you need without having to compile anything by hand.

If you're into that though, all the better. You can probably skip over the rest of this section.

<h3 id="valet">Mac: Laravel Valet <span class="bg-pink py-px px-1 rounded text-sm uppercase text-white">Our Favorite!</span></h3>

[Laravel Valet][valet] is a development environment for Mac minimalists. No Vagrant, No Apache, No Nginx, No need to manually edit hosts file. It simply maps all the subdirectories in a "web" directory (such as `~/Sites`) to `.test` or `.localhost` domains. You can even share your sites publicly using local tunnels with a single command. We use it ourselves and it's brilliant. It is also [available for Linux](https://cpriego.github.io/valet-linux/)

> Valet only supports running in web root out of the box, but if you need to run in a subdirectory you should check out the LionsMouth [Valet Driver][valet-driver]

### Mac: MAMP/MAMP Pro {#mamp}

If you prefer a GUI interface, the latest version of [MAMP and MAMP Pro][mamp] comes pre-loaded with Apache, up to date versions of PHP and all the modules you need. Download, install, and go.

### Windows: WAMP {#wamp}

If you happen to be from the Microsoft camp, we hear [WAMP](http://www.wampserver.com/en/) is a good choice, and pretty similar to MAMP. We don't do Windows so we can't vouch for it personally.

### Laravel Homestead {#homestead}

Prefer a virtual environment? You’re in luck, [Laravel Homestead][homestead] is a pre-packaged Vagrant "box" that provides you a wonderful development environment without requiring you to install PHP, HHVM, a web server, or any other server software on your local machine. No more worrying about messing up your operating system! If something goes wrong, you can destroy and re-create the box in minutes.

Homestead runs on any Windows, Mac, or Linux system, and includes the Nginx web server, PHP 5.6, MySQL, Postgres, Redis, Memcached, Node, and all of the other goodies you need to develop amazing Laravel applications.

> Note: Homestead is not a fast local dev environment for applications that manage lots of small files due to NFS sync delays. For best results avoid file sharing and run Statamic directly in your VM.

You can try enabling NFS to speed up Homestead. In your `homestead.yaml`, add `type: "nfs"` to your `folders` array.

``` .language-yaml
folders:
  - map: ~/Code
    to: /home/vagrant/Code
    type: "nfs"
```

### CLI Server {#cli-server}

Statamic doesn't support Laravel's native `serve` command, but you _can_ use PHP's CLI Server (for which the `serve` command is just a wrapper for). You must
specify `statamic/server.php` to use as a router file.

``` .language-bash
$ php -S localhost:3000 statamic/server.php

PHP 5.6.10 Development Server started at Thu Jan 21 10:30:00 2016
Listening on http://localhost:3000
```

## Command Line Installation {#command-line-installation}

First, [download the Statamic CLI](https://github.com/statamic/cli#installing-the-package) tool using Composer. You only have to do this once.

Once installed, the `statamic new` command will create a fresh Statamic site in the directory you specify.

``` .lang-bash
statamic new awesome-site
```

This will download and install Statamic into the `awesome-site` directory.


## Manual Installation {#manual-installation}

Grab the latest version of [statamic](https://statamic.com/try) and let's do this.

### Step 1: Unzip files into your webroot {#step-1-files}

Unzip your Statamic package into your web root. You'll see the following folders and files:

```language-files
webroot/
|-- assets/
|-- local/
|-- site/
|-- statamic/
|-- index.php
|-- please
|-- robots.txt
|-- sample.gitignore
|-- sample.nginx.conf
|-- sample.htaccess
|-- sample.web.config
```

#### Running in a subdirectory {#subdir}

Subdirectory installs are a little special. We've dedicated a whole page to it!

[How to run Statamic in a subdirectory](/knowledge-base/subdirectory-installation)

### Step 2: Set permissions {#permissions}

Every Statamic instance needs full write access to the following 4 directories recursively (e.g. all their subfolders and files).

- `site`
- `local`
- `statamic`
- `assets`

In order to have write access, the necessary permissions depend on which system user PHP is running as and which user owns the files and folders. Here are some recommendations. When in doubt (or on dev), throw caution to the wind with `777`:

- If they are the same user, use `744`.
- If they are the same group, use `774`.
- If they are neither the same user nor in the same group, or if you're tired of messing with this and you're still in dev, just use `777`.

> Apply the permissions recursively so Statamic can write where it needs to.

The simplest way to apply the permissions is to use the following command. (You may need to use `sudo`)

``` .language-bash
chmod -R 777 site local statamic assets
```

### Step 3: Configure URL rewrites {#rewrites}

Like most (if not all) PHP applications, all page requests are run through a single `index.php` file called a "front controller". This allows the page to be dynamically displayed from the CMS.

Technically this means all your URLs are actually `/index.php/about` but they will get rewritten to `/about`. It's better for SEO, and the `index.php` just looks silly, so you should remove it.

> Note: Statamic's default configuration assumes URL rewrites are enabled. If you notice that images and/or subpages aren't loading, it's probably because your server environment does not have URL rewrites configured. On Apache, you'll need to [enable htaccess](https://www.digitalocean.com/community/tutorials/how-to-use-the-htaccess-file), and on nginx you'll need to set up `try_files` rules.

#### Apache {#apache}

Make sure you have `mod_rewrite` enabled and rename the `sample.htaccess` file to `.htaccess`, taking special care to ensure that it's in the same directory as your `index.php` file. They're best friends, don't separate them.

If this doesn't work, you'll need to tell Apache to activate/respect your `.htaccess` file. [Here's an article](https://www.digitalocean.com/community/tutorials/how-to-use-the-htaccess-file) on how to do it.

#### Nginx {#nginx}

Grab the settings from `sample.nginx.conf` and customize them as necessary. Nginx is a bit less "set it and forget it" than Apache, making further server configuration beyond the scope of this guide.

#### IIS {#iis}

We don't use Windows ourselves, but we've been told the included `sample.web.config` works. Do your thing.

#### Disabling URL Rewrites {#disable-rewrites}

If for whatever reason you can't or don't want to use URL rewriting, you can configure Statamic to leave `index.php` in your URLs.

- Open `index.php` and change `$rewrite_urls` to `false`.
- Open `site/settings/system.yaml`, and add `index.php` to the `url` in the `locales` array.

### Step 4: Set Server Access Permissions {#access-permissions}

Next, you'll want to be sure you're using the proper access permission rules in your server config. Refer to our included sample htaccess, nginx, or web.conf files and code comments for more details.

### Step 5 (optional): Run the Installer {#installer}

Technically there is no "install" process for Statamic, but we have a little tool that will check your environment for all the necessary requirements, file permissions, locales, and even help you get your first User created. Head to `/installer.php` and let it take care of the rest for you.

**Once you're done, delete `installer.php`.**

If you don't want to (or can't for some reason) use the GUI installer, here's what to do yourself:

- [Create an admin user][create-user]
- Log into the Control Panel at `{yoursite}.{tld}/cp`
- Visit your System settings (/cp/settings/system) and set/confirm your basic site settings

### There is no step 6. {#no-step-6}

You're probably done. Now for some things to note, and a few additional steps for running above webroot and multilingual sites.

#### About that License Key and Dev Mode {#dev-mode}

If you don't have a license key, that's okay! You can use Statamic in developer mode for as long as you'd like in your local environment. Just be sure to [purchase](https://store.statamic.com) and add the key to your system config before you launch, otherwise you won't be able to access your control panel.

#### Site URL and Permalinks {#site-url}

Out of the box, Statamic will only use relative URLs as a way to get things going smoothly. However if you want to use permalinks (full URLs that
include your domain) you'll need to adjust it in `site/settings/system.yaml` in the `locales` array. Change the `url` from a relative
to a full URL like `http://mysite.com/`. If you ran the [installer](#installer), you've probably already done this.

#### Moving Statamic Above Webroot (optional) {#above-webroot}

For extra security you can [move your system files](/knowledge-base/running-above-webroot) above webroot. This prevents system files from potentially being accessed through a browser.

#### Multilingual Sites {#multilingual}

If you'd like to support multiple languages, head over to [Localization][localization] for a few additional steps.

[laravel]: http://laravel.com
[forge]: http://forge.laravel.com
[do]: https://www.digitalocean.com/?refcode=6469827e2269
[mamp]: https://www.mamp.info/en/
[valet]: http://laravel.com/docs/valet
[homestead]: http://laravel.com/docs/homestead
[localization]: /localization
[v1-upgrade]: /migrating
[cp]: /control-panel
[valet-driver]: https://github.com/LionsMouthDigital/Laravel-Valet-Drivers

#### Clearing default site data

If you have used Statamic before and know what you're doing, you can choose to clear the default content, storage, settings, theme, assets, and/or users.

Run `php please clear:site` inside your Statamic directory to initiate the clear process. You will be prompted to choose which data you would like to clear. If you want to delete everything and start fresh, it's also possible to skip the prompts and wipe everything using `php please clear:site --force`.