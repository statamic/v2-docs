---
title: How do Filesystems work?
overview: "A flat file CMS would be nothing without flexible control of files. Learn how to configure and leverage Statamic's filesystems."
id: b03df2fb-db07-4c9b-9707-d90dd7311533
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
---
## Filesystems?
Yes. Plural. Statamic has several self-contained filesystems powered by [Flysystem](http://flysystem.thephpleague.com/). By breaking the application structure into siloed locactions, each with it's own filesystem, we gain the ability to physically put these files in more locations (Amazon S3 for example) than a single web directory. What you do with that ability is up to you. We feel compelled to tell you there's nothing wrong with defaults. Don't succumb to complex configuration simply to step up your Tinder game. It probably won't impress anybody.

Statamic has 5 core filesystems, each with it's own 'root':

- Server
- Content
- Storage
- Users
- Asset Containers

## Smartish defaults
We make some out of the box assumptions about your configuration, all of which can be changed.

Here's a simplified version of what you see in a freshly unzipped Statamic installation:

``` .language-files
/ /
|-- assets/
|-- local/
|-- site/
|   |-- content/
|   |-- storage/
|   |-- users/
|   `-- themes/
|-- statamic/
`-- index.php
```

### Content and Storage
The content and storage filesystems' roots will be `site/content` and `site/storage`, respectively.

A common use case for moving something outside of the default webroot is to have a publicly accessible repo for _just_ content, keeping the rest of the site private. In fact, these docs do that very thing.

### Users
The users filesystem's root will be `site/users`.

Since you may potentially have many users, or wish the keep your celebrity gossip blogger's identity secret, you might want to keep them separate from your repo.

### Themes
The themes filesystem's root will be `site/themes`.

The theme folder needs to be publicly accessible, so we've added a `url` value here too. This will let Statamic know
how to access that folder from the browser. We'll use that for links to stylesheets, etc.

### Asset containers
Each asset container gets their own filesystem. Now you can put them on S3, for example.

The sample asset container included with Statamic will have it's filesystem root set to `assets`.

### Server
The server filesystem is used for finding any files that aren't moveable, those that need to always be on your server for the application to function. We'll use this filesystem for accessing settings, application files, and whatever else we didn't feel like typing out here because it doesn't and shouldn't matter to you.

The server filesystem's root will _always_ be the folder above your `statamic` directory.


## Moving things around

Now that you know what the filesystems are and that you can move them around, you'll want to know _how_.

The defaults settings are stored in your `site/settings/system.yaml` file like so:

``` .language-yaml
filesystems:
  content:
    driver: local
    root: site/content
  storage:
    driver: local
    root: site/storage
  users:
    driver: local
    root: site/users
  themes:
    driver: local
    root: site/themes
    url: /site/themes
```

If you don't see this in your `system.yaml` just copy these settings in or use the Control Panel.

To relocate a filesystem, simply change the `root` variable to one of the following:

  - A path relative to the main filesystem root. (eg. `site/content`)
  - An absolute path. (eg. `/var/www/example.com/content`)
  - An absolute path with an [interpolated environment variable](/knowledge-base/settings). (eg. `"{env‌:BASE_PATH}/content"`)

\* Asset containers can be modified in their respective `container.yaml` file or CP pages.


## Accessing files

To manipulate each filesystem, we'll use the `Statamic\API\File` or `Statamic\API\Folder` classes and specify a
`disk` depending on which one we want to access. If a disk isn't specified, the main filesystem will be used.

When passing paths into methods, you'll want to treat them as relative to the filesystem you're using.

``` .language-php
use Statamic\API\File;

$home     = File::disk('content')->get('pages/index.md');
$document = File::disk('storage')->get('documents/important.pdf');
$admin    = File::disk('users')->get('admin.yaml');
$css      = File::disk('themes')->get('redwood/css/style.css');
$system   = File::get('site/settings/system.yaml');
```

You can learn more about manipulating [files](/addons/classes/api/file) and [folders](/addons/classes/api/folder)
over at the [addon docs](/addons).
