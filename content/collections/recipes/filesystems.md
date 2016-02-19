---
title: Filesystems
overview: "Being a file-based CMS, there's times that we need to access files. Well, pretty much all the time. There are a number of ways to set up your filesystem to play nice with different setups."
id: b03df2fb-db07-4c9b-9707-d90dd7311533
---
## Filesystems?
You might be wondering why we're using the plural form of the word. That's because different parts of Statamic
consider themselves to be their own self-contained filesystem.

There are a handful of filesystems in Statamic:

- The main filesystem
- Content
- Storage
- Users
- Asset containers

Depending on what data we're trying to access, we'll call on the appropriate filesystem. Each filesystem will have its
own 'root'. This allows you to have different parts of your Statamic installation in different locations, whether that's
another folder on your server or even on a cloud-based service like Amazon S3.

## What's in the box?
Out of the box, Statamic makes some assumptions about your setup, which of course, you're able to change.

Here's a simplified version of what you see in a freshly unzipped Statamic installation:

``` .language-files
/ /
|-- assets/
|-- local/
|-- site/
|   |-- content/
|   |-- storage/
|   `-- users/
|-- statamic/
`-- index.php
```

Now, keep in mind the structure above and the filesystems mentioned earlier:

### Content and Storage
The content and storage filesystems' roots will be `site/content` and `site/storage`, respectively. We'll look in here
any time we need to deal with content or storage.

A common use-case is to have a publicly accessible repo for just content, while keeping the rest of the site
in a private repo. Since the content folder is treated as its own filesystem, you can change its location
or even (eventually) keep it on a cloud service like Amazon S3 or Dropbox.

### Users
The users filesystem's root will be `site/users`.

Since you may potentially have many users, you might want to keep then separate from your repo. Or, who knows what you
want to do with it. If you have a good reason for moving them, you can!

### Asset containers
Each asset container gets their own filesystem, which again means they may be located on your server or on a cloud
service.

The sample asset container included with Statamic will have it's filesystem root set to `assets`.

### Main filesystem
The main filesystem is used for finding any files that aren't moveable. These files will always be on your server.
We'll use this filesystem for accessing settings, Statamic core files, and really anything that wasn't mentioned above.

The main filesystem's root will _always_ be the folder above your `statamic` directory.


## Moving around

Now that you know what the filesystems are, you'll want to know how to move them.

The defaults mentioned above* are stored in your `site/settings/system.yaml` file like so:

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
```

If you don't see this in your `system.yaml`, just copy this in, or hit save from within the CP.

To relocate a filesystem, simply change the `root` variable to one of the following:

  - A path relative to the main filesystem root. (eg. `site/content`)
  - An absolute path. (eg. `/var/www/example.com/content`)
  - An absolute path with an [interpolated environment variable](/docs/recipes/settings). (eg. `"{env‌:BASE_PATH}/content"`)

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
$system   = File::get('site/settings/system.yaml');
```

You can learn more about manipulating [files](/addons/classes/api/file) and [folders](/addons/classes/api/folder)
over at the [addon docs](/addons).
