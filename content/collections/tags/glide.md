---
title: Glide
overview: Manipulate Asset images on the fly.
parameters:
  -
    name: src
    type: string
    description: The ID of the asset when _not_ using the shorthand syntax. (Use the shorthand syntax if you can, it's nicer.)
  -
    name: field
    type: tag part
    description: 'The name of the field containing the asset ID when using the shorthand syntax. This is not actually a parameter, but part of the tag itself. For example, `{{ glide:hero_image }}`.'    
  -
    name: filename
    type: string
    description: >
      Optionally prepends a filename to the end of the URL. See [_Vanity Filenames_](#vanity-filenames).
  -
    name: width
    type: integer
    description: Width in pixels
  -
    name: height
    type: integer
    description: Height in pixels
  -
    name: square
    type: integer
    description: >
      A shortcut for setting `width` and
      `height` to the same value.
  -
    name: fit
    type: string *crop_focal*
    description: >
      How the image should fit into the target
      dimensions. Options are `contain`,
      `max`, `fill`, `stretch`, `crop`, `crop-x-y`,
      and `crop_focal`. See the [_Focal Crop_](#focal-crop) section
      for more details.
  -
    name: crop
    type: string
    description: 'Crops the image to specific dimensions prior to any other resize operations. Required format: `width,height,x,y`'
  -
    name: orient
    type: mixed
    description: >
      Rotates the image. Accepts `auto`, `0`,
      `90`, `180` or `270`. Default is `auto`.
      The `auto` option uses Exif data to
      automatically orient images correctly.
  -
    name: brightness
    type: integer
    description: >
      Adjusts the image brightness. Use values
      between `-100` and `+100`, where `0`
      represents no change.
  -
    name: contrast
    type: integer
    description: >
      Adjusts the image contrast. Use values
      between `-100` and `+100`, where `0`
      represents no change.
  -
    name: gamma
    type: integer
    description: >
      Adjusts the image gamma. Use values
      between `0.1` and `9.99`.
  -
    name: sharpen
    type: integer
    description: >
      Sharpen the image. Use values between
      `0` and `100`.
  -
    name: blur
    type: integer
    description: >
      Adds a blur effect to the image. Use
      values between `0` and `100`.
  -
    name: pixelate
    type: integer
    description: >
      Applies a pixelation effect to the
      image. Use values between `0` and
      `1000`.
  -
    name: filter
    type: string
    description: >
      Applies a filter effect to the image.
      Accepts `greyscale` or `sepia`.
  -
    name: quality
    type: integer *90*
    description: >
      Adjusts the quality of the image. Use values between `0` and `100`.
id: b70a3d9a-6605-446e-b278-de99ba561fe0
---
## Example {#example}


We have an image's ID saved in the YAML, and we want to resize it to 300x200, fit it inside the area by cropping it, and apply a sepia filter.

``` language-yaml
image: "380dc8d9-481c-4d18-9162-ecd5688f98a8"
```

```
{{ glide src="{image}" width="300" height="200" fit="crop" filter="sepia" }}

<!-- shorthand syntax: -->
{{ glide:image width="300" height="200" fit="crop" filter="sepia" }}
```

``` .language-output
/img/380dc8d9-481c-4d18-9162-ecd5688f98a8?w=300&h=200&fit=crop&filt=sepia&s=3982hf983f2mf90r23
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


## Vanity Filenames {#vanity-filenames}

The Glide URL is not really pretty, nor SEO friendly. The URL contains the ID of the image asset. For example: `/img/cbffccd0-ae6f-11e5-a837-0800200c9a66?w=20`.

Specifying `filename="bacon.jpg"` would change the URL to `/img/cbffccd0-ae6f-11e5-a837-0800200c9a66/bacon.jpg?w=20`.

The additional segment is purely cosmetic in terms of Statamic functionality (hence 'vanity'). However now if you were to right click and save the image, it would name it `bacon.jpg`. It would also be more recognizable to search engines as a photo of bacon rather than a photo of a `cbffccd0-ae6f-11e5-a837-0800200c9a66`.
