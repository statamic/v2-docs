---
title: Assets
id: 28bf911b-8f47-489b-8040-37607733b270
overview: >
  If we're going to have assets around, we have to be concerned about them and take care of them. This is a happy place. Let's make friends with our assets. Everyone needs a friend.
---
## Overview {#overview}

---

**Assets have been revamped with the release of Statamic 2.5.**  
These docs assume you're using at least version 2.5.  
If you're upgrading, [check out the differences below](#statamic-2-5).

---

Assets are files, whether images, videos, documents, zip files, or anything else within reason you want to upload, manage, and display on your site. Assets are grouped into Containers, allowing you to keep your files organized.

![Some possible Assets](/assets/img/other/assets-filetypes.png)

## Containers {#containers}

Each asset container has its configuration file located in `site/content/assets`.  
For example, `site/content/assets/main.yaml` where `main` is the ID.

The container holds the details about its root location. This can be a folder on the server, or credentials to an Amazon S3 bucket. See the [Asset Container Drivers](#asset-container-drivers) for more detail.

The container also holds any additional asset data.

You may find it easier to create an asset container using the CLI with this command:

``` .language-cli
php please make:asset-container
```

You may also use the wizard in the Control Panel found at `Configure > Content > Assets`.


## Managing Assets {#managing-assets}

Any files placed within an asset container's directory will be recognized as assets. You may add as many subfolders as necessary to stay organized.

You can reference assets within your content by using its URL.

For example:

``` .language-yaml
title: Vacation to Italy
photos: 
  - /assets/vacations/italy/trevi-fountain.jpg
  - /assets/vacations/italy/colosseum.jpg
```

The exception to this is assets located in private containers. Their IDs should be saved within content. See [Private Assets](#private-assets) for more detail.

If you're using the Control Panel, the [Asset fieldtype][assets-fieldtype] can be used to browser, select, upload, reorder, and remove assets from your content.

![Asset Fieldtype](/assets/img/other/cp-asset-fieldtype.png)

Moving or renaming assets (either manually or through the Control Panel) will *not* automatically update your content, which may result in broken links.


## Templating {#templating}

Since assets live in your content with their URLs, you can simply iterate over an array:

```
{{ photos }}
  <img src="{{ value }}" />
{{ /photos }}
```

``` .language-output
<img src="/assets/vacations/italy/trevi-fountain.jpg" />
<img src="/assets/vacations/italy/colosseum.jpg" />
```

Of course, if your YAML contains a string instead of an array, or you'd like to target a specific item in the array, you can do that too:

``` .language-yaml
photo: /assets/jerky.jpg
photos:
  - /assets/bacon.jpg
  - /assets/whisky.jpg
```

```
{{ photo }}
{{ photos:1 }}
```

``` .language-output
/assets/jerky.jpg
/assets/whisky.jpg
```

For more control, you may use the [Assets (or Asset) Tag][assets-tag].

These tags will convert your URL references to actual Asset objects, which can contain more data for you to use within your templates.

```
{{ assets:photos }}
  <img src="{{ url }}" alt="{{ alt }}" />
{{ /assets:photos }}
```

``` .language-output
<img src="/assets/vacations/italy/trevi-fountain.jpg" alt="The Trevi Fountain" />
<img src="/assets/vacations/italy/colosseum.jpg" alt="The Colosseum" />
```


## Additional asset data {#additional-data}

You may attach data (fields) to an asset just like you would with entries or pages. This is useful for things like alt text.

In the example above, we reference `{{ alt }}` within the `assets` tag pair. The value comes from what we've defined in the additional data.

Additional asset data is held within the asset container file, in an `assets` array. The relative path of the asset should be used as the key, with an array of data as the value.

Here's what a container might look like:

``` .language-yaml
title: Main Assets
path: assets
url: /assets
assets:
  vacations/italy/trevi-fountain.jpg:
    alt: The Trevi Fountain
  vacations/italy/colosseum.jpg:
    alt: The Colosseum
```

When moving or renaming an asset, be sure to update its key in this array.  
If you're using the Control Panel, this will be done automatically for you.

Note: If you don't need any additional asset data, simply do nothing! (That's one of the changes that landed in [Statamic 2.5 - check it out](#statamic-2-5).)


## Private Assets {#private-assets}

Sometimes it's handy to store assets that shouldn't be freely accessible through the browser.

This means you should put the container somewhere above the webroot, which in turn means that it doesn't have a URL, nor will any of its assets.

You may still reference and access private assets, by using a couple of extra steps.

Since private assets have no URLs, you must reference them with their IDs, which is the container ID and the path joined by a double colon.

``` .language-yaml
top_secret_stuff: 
  - confidential::docs/evil-lair-blueprints.jpg
```

However now you can't simply iterate over `top_secret_stuff`, since it doesn't contain any real URLs. To combat this, you'll need to use the [Glide Tag][glide-tag] to transform it and place in a public location. You can even just use the glide tag without any parameters.

```
{{ top_secret_stuff }}
  <img src="{{ glide:value }}" />
{{ /top_secret_stuff }}
```

Of course, private assets may also have [their own data](#additional-data) which you can access using the [Assets tag][assets-tag] as mentioned earlier.


## Image Manipulation {#image-manipulation}

Image assets may be manipulated in a number of ways. Size, color, quality, etc. We use [Glide](http://glide.thephpleague.com/) to power our image transformations.

Statamic provides a URL-based manipulation API through Glide. Everything runs through a manipulation route (which by default is `img`.)

For example, take this URL:

```
/img/assets/vacations/italy/trevi-fountain.jpg?w=100&h=100
```

- Statamic sees its an image manipulation URL because it starts with `/img/`
- It sees the path to an image: `assets/vacations/italy/trevi-fountain.jpg`
- It will manipulate and output an image based on the parameters. `?w=100&h=100`.

The appropriate way to generate these URLs in your templates is by using the [Glide Tag][glide-tag].

```
{{ glide src="/assets/vacations/italy/trevi-fountain.jpg"
         width="100" height="100" }}
```

The Glide tag can also accept asset IDs for private assets, and even external URLs. It's quite handy!  
Visit the [Glide Tag's docs][glide-tag] for more details.

### Manipulation Presets {#manipulation-presets}

You may define named presets to simplify your templates.

Within `site/settings/assets.yaml`, you can add a list of presets with the keys as their names, and the values as an array of raw [Glide API parameters](http://glide.thephpleague.com/1.0/api/quick-reference/). For example:

``` .language-yaml
image_manipulation_presets:
  sml:
    w: 200
    h: 200
  tall:
    w: 200
    h: 600
```

Whenever you upload a new image asset through the control panel, all of that image's presets will be created automatically.

To generate the presets for existing assets, you can run `php please assets:generate-presets`.

#### Queueing Presets {#queueing-presets}

As mentioned earlier, presets are generated when you upload the assets. Depending on how many presets you have, and what's involved in each one, this may take a long time or even cause the request to run out of memory.

You may place the presets in a queue so they can run in the background.

- Ensure [Redis][redis] is installed and running on your server.
- Add `QUEUE_DRIVER=redis` to your `.env` file. [What's an .env file?](/environments).
- Run `php please queue:listen`



## Asset Container Drivers {#asset-container-drivers}

The examples above assume you are storing your assets in folders within your site. However, it’s possible to store them in other locations on your server, or even in the cloud using [Amazon S3](https://s3.amazonaws.com/).

Within each container’s configuration file, you may specify the `driver` and its various options.

### Local Filesystem {#local-driver}

When a container doesn’t have a driver specified, Statamic will assume it uses the `local` driver.

``` .language-yaml
title: Local Assets
path: path/to/asset/folder
url: /url/of/asset/folder
```

The path may be:

- A path relative to the main filesystem root. (eg. `path/to/assets`) This can be inside or outside your site’s root folder.
- An absolute path. (eg. `/var/www/example.com/assets`)
- An absolute path with an interpolated environment variable. (eg. `"{env‌:BASE_PATH}/assets"`)

As mentioned above, the url should be the location of the asset folder. If it’s located outside of the webroot, it’s inaccessible so you can just leave it blank. A container without a URL will be considered [private](#private-assets).

### Amazon S3 {#s3-driver}

To enable S3 assets in a container, you should set up your container configuration file like so:

``` .language-yaml
title: S3 Assets
driver: s3
key:     # Access Key ID
secret:  # Secret Access Key
bucket:  # The bucket name
region:  # The region code (eg. us-west-1)
path:    # A subfolder of the bucket, if you'd like
cache:   # A cache time in seconds. More info below.
```

The region codes can be tough to remember. [Here’s a list of them.](http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region)


#### S3 Filesystem Caching {#s3-caching}

Amazon S3 requires API calls to retrieve information from Amazon. With a lot of files, and a lot of actions, this can very easily become slow.

The filesystem may be cached with [Redis][redis] to prevent repeated API calls.

- Ensure your server has Redis installed and running.
- Set the `cache` key in your container configuration file to the number of seconds a particular folder's contents should be cached.


## Statamic 2.5 Changes {#statamic-2-5}

Statamic 2.5 changed a lot of how the asset system works. If you're coming from an earlier version, here's what you'll want to know:

- You no longer need to add each asset to a YAML file. If the file exists in the folder, it exists.
- The containers have moved from `site/storage/assets/[id]/container.yaml` to `site/content/assets/[id].yaml`.
- Instead of a separate `folder.yaml` file to track assets in each subfolder, all additional asset data is held in the container file.
- You now reference assets in your content with URLs instead of IDs.
- Asset IDs technically still exist, but now they are just `[container id]::[path to file]`. Not a crazy UUID.
- Asset IDs are used in your content for private containers.


[glide-tag]: /tags/glide
[assets-tag]: /tags/assets
[assets-fieldtype]: /fieldtypes/assets
[redis]: https://redis.io/