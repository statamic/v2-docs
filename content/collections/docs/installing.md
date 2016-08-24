---
title: Installing
id: a7668389-57ab-47aa-b26f-6d2299b5b534
---

## The Screencast Version {#screencast}

Visual learner? Watch how simple it is to install Statamic.

<iframe src="https://player.vimeo.com/video/165632057?title=0&byline=0&portrait=0" width="800" height="450" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>


## Requirements {#requirements}

Statamic has a few system requirements. Not all servers are created equal and not all hosts play nice. Statamic v2 is built on [Laravel][laravel] so as a rule of thumb, anywhere Laravel runs, Statamic runs. You can even use [Forge][forge] + [Digital Ocean][do]. We do, and we love it. Anyway, here's what you're going to need.

### Server Requirements {#server-requirements}

- A web server: Apache, Nginx, or IIS
- PHP >= 5.5.9

### Required PHP Extensions {#required-php-extensions}

These should all be compiled in PHP 5.5.9+ by default but we find it's best to be thorough.

- [OpenSSL](http://php.net/manual/en/book.openssl.php)
- [Multibyte String](http://php.net/manual/en/book.mbstring.php)
- [Tokenizer](https://secure.php.net/manual/en/book.tokenizer.php)
- [JSON](https://secure.php.net/manual/en/book.json.php)
- [PCRE](http://php.net/manual/en/book.pcre.php)

### Optional {#optional}

- [GD](http://php.net/manual/en/book.image.php) or [ImageMagick](http://php.net/manual/en/book.imagick.php)
- [Composer](https://getcomposer.org/)


## Suggested Development Environments {#dev-environments}

You can (and probably should) run Statamic _locally_ while you develop your site. There are a number of solutions that give you all the tools you need without having to compile anything by hand. But if you're into that, all the better. You can probably skip over the rest of this section.

### Mac: MAMP/MAMP Pro

The latest version of [MAMP and MAMP Pro][mamp] comes pre-loaded with Apache, PHP 5.5.9 and all the modules you need. Download, install, and go.

### Mac: Laravel Valet

[Laravel Valet][valet] is a development environment for Mac minimalists. No Vagrant, No Apache, No Nginx, No /etc/hosts file.
You can even share your sites publicly using local tunnels. We use it, it's brilliant.

_Note: Valet supports an out-of-the-box Statamic installation. Subdirectory installs don't work at the moment._

### Windows: WAMP

If you happen to be of the Microsoft persuasion, [WAMP](http://www.wampserver.com/en/) is a good choice, and pretty similar to MAMP. So we hear.

### Laravel Homestead

Prefer a virtual environment? You’re in luck, [Laravel Homestead][homestead] is a pre-packaged Vagrant "box" that provides you a wonderful development environment without requiring you to install PHP, HHVM, a web server, or any other server software on your local machine. No more worrying about messing up your operating system! If something goes wrong, you can destroy and re-create the box in minutes.

Homestead runs on any Windows, Mac, or Linux system, and includes the Nginx web server, PHP 5.6, MySQL, Postgres, Redis, Memcached, Node, and all of the other goodies you need to develop amazing Laravel applications.

_Note: Homestead is not a **fast** local dev environment for applications that leverage lots of small files due to NFS sync delays. It's best to not use file sharing and run Statamic directly in your VM for best results._

You can try enabling NFS to speed up Homestead. In your `homestead.yaml`, add `type: "nfs"` to your `folders` array.

``` .language-yaml
folders:
  - map: ~/Code
    to: /home/vagrant/Code
    type: "nfs"
```

### CLI Server

_This method isn't recommended for any long-term development._

To get started very quickly, you can use Laravel's built-in server using our `please` command.

``` .language-bash
$ php please serve

Laravel development server started on http://localhost:8000/
```

Alternatively, you can use the plain PHP version (which the `please` command uses behind the scenes). You must
specify the `statamic/server.php` to use as a router.

``` .language-bash
$ php -S localhost:3000 statamic/server.php

PHP 5.6.10 Development Server started at Thu Jan 21 10:30:00 2016
Listening on http://localhost:3000
```

Note that some users are reporting issues with both of these options.


## Installing {#installing}

Grab the latest version of [statamic.com](http://statamic.com) and let's get to it.

### Step 1: Drop the files {#step-1-files}

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

Gut check time. Do you want to run in a subdirectory for the right reason? Using Statamic in a `blog` subdirectory in an existing site is one such reason. Not feeling like setting up a virtual host isn't. We can't stop you, but if you plan to run the site in webroot in production, you should do the same thing in development.

Professional advice given, you'll need to do this:

- Open `index.php` and change `$site_root` from `"/"` to `"/your_subdirectory/"`
- Open `site/settings/system.yaml` and change the URL from `/` to `/your_subdirectory/`

Good to go.

### Step 2: Set permissions {#permissions}

Statamic needs to be able to write to the 4 included folders, and their subfolders.

- `site`
- `local`
- `statamic`
- `assets`

In order to have write access, the necessary permissions depend on which system user PHP is running as and which user owns the files and folders. Here are some recommendations. When in doubt, bump it up:

- If they are the same user, use `744`.
- If they are the same group, use `774`.
- If they are neither the same user nor in the same group, or if you're tired of messing with this, just use `777`.

> Apply the permissions recursively so Statamic can write where it needs to.

The simplest way to apply the permissions is to use the following command. (You may need to prefix with `sudo`)

``` .language-bash
chmod -R 777 site local statamic assets
```

### Step 3: Configure URL rewrites {#rewrites}

Like most (if not all) PHP applications, all page requests are run through a single `index.php` file called a "front controller". This allows the page to be dynamically displayed from the CMS.

Technically this means all your URLs are actually `/index.php/about` but they will get rewritten to `/about`. It's better for SEO, and the `index.php` just looks silly, so you should remove it.

_Note: Out of the box, Statamic will assume you will be using URL rewrites. If you notice only your home page working, it's usually because URL rewrites aren't configured. Either set them up, or read how to disable them at the end of this section._

Most decent servers will support URL rewriting in some way. Choose your method below:

#### Apache

Make sure you have `mod_rewrite` enabled and rename the `sample.htaccess` file to `.htaccess`, taking special care to ensure that it's in the same directory as your `index.php` file. They're best friends, don't separate them.

If this doesn't work, you're most likely someone who doesn't follow defaults and runs your own flavor of configuration. Google is your friend. If you're _really_ stuck, we'll do our best to [help you](https://statamic.com/support).

#### Nginx

Grab the settings from `sample.nginx.conf` and customize them as necessary. Nginx is a bit less "set it and forget it" than Apache, making further server configuration beyond the scope of this guide.

#### IIS

We don't use Windows ourselves, but we've been told the included `sample.web.config` works. Do your thing.

#### Disabling URL Rewrites

If for whatever reason you can't or don't want to use URL rewriting, you can configure Statamic to leave `index.php` in your URLs.

- Open `index.php` and change `$rewrite_urls` to `false`.
- Open `site/settings/system.yaml`, and add `index.php` to the `url` in the `locales` array.

### Step 4: Run the Trailhead Installer {#installer}

Technically there is no "install" process for Statamic, but we have a little tool that will check your environment for all the necessary requirements, file permissions, and even help you get your first User created. Head to `/installer.php` and let it take care of the rest for you.

**Once you're done, delete `installer.php`.**

### Step 5: You're done.

Now for some extra detail...

#### About that License Key and Trial Mode

If you don't have a license key, that's okay! You can use Statamic in developer mode for as long as you'd like in your local development environment. Just be sure to [purchase](https://trading-post.statamic.com) and add the key to your system config before you launch, otherwise you won't be able to access your control panel.

#### Site URL and Permalinks

Out of the box, Statamic will only use relative URLs as a way to get things going smoothly. However if you want to use permalinks (full URLs that
include your domain) you'll need to adjust it in `site/settings/system.yaml` in the `locales` array. Change the `url` from a relative
to a full URL like `http://mysite.com/`. If you ran the [installer](#installer), you might have already done this.

#### Moving Statamic Above Webroot (optional) {#above-webroot}

For extra security you can [move your system files](/knowledge-base/running-above-webroot) above webroot. This prevents system files from potentially being accessed through a browser.

#### Multilingual Sites

If you'd like to support multiple languages, head over to [Localization](/localization) for a few additional steps.