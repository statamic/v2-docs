---
title: Happy Little Assets
sub_title: Assets
cover: /assets/img/trail-guides/assets.jpg
description: "We want happy assets. Here is freedom and absolutely no pressure, just like an angel's wing."
overview: "If we're going to have assets around, we have to be concerned about them and take care of them. This is a happy place. Let's make friends with our assets. Everyone needs a friend."
id: 11265994-38b8-4516-b89e-117ebc4e38af
---
Assets are files, whether images, videos, documents, zip files, or anything else within reason you want to upload, manage, and display on your site. Assets are grouped into Containers, allowing you to keep your files organized.

![Some possible Assets](/assets/img/other/assets-filetypes.png)

An Asset Container is a location on your filesystem. You can have multiple Containers to organize your assets however you wish. For example, you could have one container inside webroot for publicly accessible assets like images and PDFs, and one outside webroot for private assets, like zip files full of scanned images of your parent's love letters.

Each Asset Container can contain many folders and subfolders, as well as a **title** for visual display purposes. Head to the `Assets` area of the Control Panel to start creating your Containers setting yourself up for success.

![Assets in the Control Panel](/assets/img/other/cp-assets.png)

## Creating and using Assets

Each Asset gets it own unique ID so it can be referenced and related by other content. This allows an Asset's location, filename, or any bit of metadata to change without breaking that relationship and, ultimately, the URL it's accessed at.

You can use the [Asset fieldtype][asset-fieldtype] to access and use those files when managing Pages, Entries, Globals, and Users.

![Asset Fieldtype](/assets/img/other/cp-asset-fieldtype.png)

When Assets are attached to content it is the `id`, not the filename, that is saved to your content file's front-matter and made available to your templates.

``` .language-yaml
bacon_images:
  - 89jf89r32-fdsmar39ifm9-932c9m3j3
  - 2n329ofna-90fjqp49j9fr-e98rjaa82
```

## Assets all up in your templates

Now you have these ugly IDs instead of pretty filenames. Before you puff your checks and pull out your sad trombone, look at what you can now do. This is the payoff, right here.


```
{{ assets:bacon_images }}
  <img src="{{ url }}" alt="{{ alt }}" />   Size: {{ size }}
{{ /assets:bacon_images }}
```

Pass the name of your variable holding Asset ids to the [Assets tag][assets-tag] and tap right into all of the available data, just like that.

``` .language-output
<img src="/assets/applewood-smoked.jpg" alt="Applewood" /> Size: 355kb
<img src="/assets/canadian-bacon.jpg" alt="Canadian" /> Size: 125kb
```

## Image transformations

In addition to template-level flexibility, image Assets can also be manipulated with our URL based transformation API.

```
{{ assets:bacon_images }}
  <img src="{{ url }}?w=300&h=250&fit=crop" />
{{ /assets:bacon_images }}
```

You can resize, make adjustments, crop, and add effects, all on the fly in your templates.

Of course, if you can do this so can your users. To prevent a malicious visitor or -- let's face it -- a bored teenager, from overloading your server with an excessive amount of image processing, you should use the [Glide Tag][glide-tag] and Secure Mode. We've even enabled that by default.

This requires a unique key to be added to image URLs which is used to validate a match on the server and client side, resulting in the desired transformation. The Glide Tag does this for you automatically. Feel free to disable all of this in dev mode, it's frankly just more fun to play with the URL directly.

## Storage Structure

Let's take a look at how this is structured on the file system level. Once configured, you will rarely have to worry about this, but it's good know how it all works.

Let's assume we're running Statamic in an outside-webroot configuration.

``` .language-files
//
|-- public/
|   |-- assets/
|   |   |-- img/
|   |   |   |-- bass-tarts.jpg
|   |   └── pdf/
|   |       └── happy-little-resume.pdf
|   └── index.php
|-- private/
|   └── the-secret-to-perfect-bacon.pdf
|-- site/
|   └── storage/
|     └── assets/
|         |-- 90ua-f200-3af/
|         |   |-- img/
|         |   |   └── folder.yaml
|         |   |-- pdf/
|         |   |   └── folder.yaml
|         |   |-- container.yaml
|         |   └── folder.yaml
|         └── 09fd-d99a-1ka/
|             |-- container.yaml
|             └── folder.yaml
└── statamic/
```

`public` is the webroot.

`public/assets/` is the location of one of our asset containers. As this is within webroot, any assets in here will be accessible.

`private/` is the location of another container. Being outside the webroot, these files won't be publicly accessible.

`site/storage/assets/` is where all metadata for our assets will be stored. Each asset container will get their own folder here, named by the unique ID assigned to them.

## Container Configuration

An Asset Container's configurations are stored in a `container.yaml` file in the root of that directory. For example, `site/storage/assets/90ua-f200-3af/container.yaml` might look something like this:

``` .language-yaml
title: "Public Assets"
path: public/assets
url: /assets
```

`path` is the directory to the asset location, relative to the [server filesystem root][filesystems].

`url` is the base url of this location.

Then the other `container.yaml` might look like this:

``` .language-yaml
title: "Private Assets"
path: private
```

There is no URL because this container isn't publicly accessible.

Within the storage directory for each container, there will be a `folder.yaml` that corresponds to each folder in the respective container. In each `folder.yaml`, you will find an array containing the IDs and metadata for every file in the directory. It also contains data for the folder itself.

For example, the `site/storage/assets/90ua-f200-3af/img/folder.yaml` file might look like this:

``` .language-yaml
title: Images
assets:
  978fh-uf9ai-89aj3j:
    file: bass-tarts.jpg
    title: Kellogg's Frosted Bass Tarts
    alt: They're just like Pop Tarts, but full of Large Mouth Bass.
```

## Asset Drivers

The examples above assume you are storing your assets in folders within your site. However, it's possible to store them
in other locations on your server, or even in the cloud, like on Amazon S3.

Note: Your _asset files_ will be located where you specify, _not the meta data_. The meta data will always be located
in the storage folder.

Within each container's `container.yaml`, you may specify the `driver` and its various options.

### Local
When a container doesn't have a `driver` specified, Statamic will assume it uses the `local` driver.

``` .language-yaml
title: Local Assets
path: path/to/asset/folder
url: /url/of/asset/folder
```

The `path` may be:

  - A path relative to the main filesystem root. (eg. `path/to/assets`) This can be inside or outside your site's root folder.
  - An absolute path. (eg. `/var/www/example.com/assets`)
  - An absolute path with an [interpolated environment variable](/reference/recipes/settings). (eg. `"{env‌:BASE_PATH}/assets"`)

As mentioned above, the `url` should be the location of the asset folder. If it's located outside of the webroot, it's
inaccessible so you can just leave it blank.

### Amazon S3
To enable S3 assets in a container, you should set up your `container.yaml` like so:

``` .language-yaml
title: S3 Assets
driver: s3
key:     # Access Key ID
secret:  # Secret Access Key
bucket:  # The bucket name
region:  # The region code (eg. us-west-1)
path:    # A subfolder of the bucket, if you'd like
```

The region codes can be tough to remember. [Here's a list of them.](http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region)

And there you have it. Go have fun with Assets!

## In the works

These features are in the oven but aren't quite fully baked yet. If you were to eat them now, you would get a bellyache and miss 2 days of school.

- Assets will eventually be be localizable.

[asset-fieldtype]: /reference/fieldtypes/assets
[assets-tag]: /reference/tags/assets
[glide-tag]: /reference/tags/glide
[filesystems]: /reference/recipes/filesystems
