---
title: Glide
overview: Manipulate images on the fly using the wonderful [Glide](http://glide.thephpleague.com/) library.
parameters_content: |
  You may pass anything from the [Glide API](http://glide.thephpleague.com/1.0/api/quick-reference/) as a parameter.  
  For example, `{{ glide or="90" }}` will use the [orientation](http://glide.thephpleague.com/1.0/api/orientation/#orientation-or)
  API parameter.
parameters:
  -
    name: src|path|id
    type: string
    description: >
      The URL of the image when _not_ using the shorthand syntax. (Use the shorthand syntax if you can, it's nicer.)
      This also accepts asset IDs, if using [private assets](/assets#private-assets), for example.
  -
    name: field
    type: tag part
    description: 'The name of the field containing the asset ID or image path when using the shorthand syntax. This is not actually a parameter, but part of the tag itself. For example, `{{ glide:hero_image }}`.'    
  -
    name: width
    type: integer
    description: >
      Alias of the [w](http://glide.thephpleague.com/1.0/api/size/#width-w) Glide API parameter.
  -
    name: height
    type: integer
    description: >
      Alias of the [h](http://glide.thephpleague.com/1.0/api/size/#height-h) Glide API parameter.
  -
    name: square
    type: integer
    description: >
      A shortcut for setting `width` (`w`) and `height` (`h`) to the same value.
  -
    name: fit
    type: string
    description: >
      Alias of the [fit](http://glide.thephpleague.com/1.0/api/size/#fit-fit) Glide API parameter. In addition to the
      Glide fit options, Statamic also accepts `crop_focal` to automatically fit/crop to a predefined focal point.
      See the [_Focal Crop_](#focal-crop) section for more details.
  -
    name: crop
    type: string
    description: >
      Alias of the [crop](http://glide.thephpleague.com/1.0/api/crop/#crop-crop) Glide API parameter.
  -
    name: absolute
    type: 'boolean *false*'
    description: >
      When set to `true`, this tag will output the full URL rather than the default relative URL.
  -
    name: orient
    type: mixed
    description: >
      Alias of the [or](http://glide.thephpleague.com/1.0/api/orientation/#orientation-or) Glide API parameter.
  -
    name: brightness
    type: integer
    description: >
      Alias of the [bri](http://glide.thephpleague.com/1.0/api/adjustments/#brightness-bri) Glide API parameter.
  -
    name: contrast
    type: integer
    description: >
      Alias of the [con](http://glide.thephpleague.com/1.0/api/adjustments/#contrast-con) Glide API parameter.
  -
    name: gamma
    type: integer
    description: >
      Alias of the [gam](http://glide.thephpleague.com/1.0/api/adjustments/#gamma-gam) Glide API parameter.
  -
    name: sharpen
    type: integer
    description: >
      Alias of the [sharp](http://glide.thephpleague.com/1.0/api/adjustments/#sharpen-sharp) Glide API parameter.
  -
    name: pixelate
    type: integer
    description: >
      Alias of the [pixel](http://glide.thephpleague.com/1.0/api/effects/#pixelate-pixel) Glide API parameter.
  -
    name: filter
    type: string
    description: >
      Alias of the [filt](http://glide.thephpleague.com/1.0/api/effects/#filter-filt) Glide API parameter.
  -
    name: quality
    type: integer *90*
    description: >
      Alias of the [q](http://glide.thephpleague.com/1.0/api/encode/#quality-q) Glide API parameter.
id: b70a3d9a-6605-446e-b278-de99ba561fe0
---
## Examples {#examples}

### Single Images {#single-images}

We have an image's asset URL saved in the YAML, and we want to resize it to 300x200, fit it inside the area by cropping it, and apply a sepia filter.

``` language-yaml
image: "/assets/food/bacon.jpg"
```

```
{{ glide :src="image" width="300" height="200" fit="crop" filter="sepia" }}

<!-- shorthand syntax: -->
{{ glide:image width="300" height="200" fit="crop" filter="sepia" }}
```

``` .language-output
/img/assets/food/bacon.jpg?w=300&h=200&fit=crop&filt=sepia&s=3982hf983f2mf90r23
```

### Multiple Images {#multiple}

If you have a list of assets, you may want to loop over them and generate images for each one. Nothing special here, just loop over them.

``` .language-yaml
images:
  - /assets/food/bacon.jpg
  - /assets/drinks/whisky.jpg
```

Since the current iteration of the loop would be output using `{{ value }}`, you can reference that in the Glide tag like so:

```
{{ images }}
  {{ glide:value width="300" height="200" }}
{{ /images }}
```

``` .language-output
/img/assets/food/bacon.jpg?w=300&h=200&s=3982hf983f2mf90r23
/img/assets/drinks/whisky.jpg?w=300&h=200&s=3982hf983f2mf90r23
```

### Complex image paths {#complex-image-paths}

If you need the path to your image to be generated with another tag, you can't just drop that into the `path` parameter.
It would likely be confusing to read your templates – there could be parameters in parameters, oh my.

Instead, use the `glide` tag as a tag pair. The contents of the tag will be used as the `src`.

```
{{ glide width="300" height="200" fit="crop" filter="sepia" }}
  {{ theme:img src="photo.jpg" }}
{{ /glide }}
```

``` .language-output
/img/site/themes/your-theme/img/photo.jpg?w=300&h=200&fit=crop&filt=sepia&s=3982hf983f2mf90r23
```


## Focal Crop {#focal-crop}

When using the `fit` parameter, you may choose to crop to a focal point. You may specify the
two offset percentages: `crop-x%-y%`.

For example, `fit="crop-75-50"` would crop the image and make sure that the point at 75% across
and 50% down would be the focal point.

Rather than specifying the offsets, you may use `crop_focal` to use the asset's saved focal point.

A Statamic image asset can be assigned a percentage based focal point. You can do this by editing an
asset and defining the focal point using the UI. Or, you may add `focus: x-y` to the asset's metadata.

When using `crop_focal` and an asset doesn't have a focal point set, it will crop from the center.

Note: All Glide generated images are cropped at their focal point, unless you disable the _Auto Crop_
setting. This happens even when you don't specify a `fit` parameter. You may override this behavior
per-image/tag by specifying the `fit` parameter as described above.


## Serving Cached Images Directly {#serving-cached-images}

Glide brings you some pretty nifty on-the-fly URL manipulations. The default behaviour of the Glide tag is to simply output a URL. When that URL is
visited, Glide analyses the URL and manipulates an image. However, if you have a lot of assets in your site and a lot of them on a page, time for each
request can soon start to add up.

It is possible to "reverse" this behavior and to simply generate static images. Your server will load the images directly instead of handing the work
over to Glide each time. If you are familiar with Statamic v1, this can be thought of similar to the "Transform" tag.

In `Configure > Settings > Assets`, or `site/settings/assets.yaml`:

``` .language-yaml
# Enable or disable the feature
image_manipulation_cached: true

# The folder containing the manipulated images.
# If you're running above webroot, this might be something like public/img
image_manipulation_cached_path: img

# The URL to the folder
image_manipulation_route: img
```
