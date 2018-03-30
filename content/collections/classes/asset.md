---
class: Statamic\Assets\Asset
title: Asset
id: 2ad8dfad-17d0-46b8-8356-87f14f9081f6
---
Inheritance: [Data](/addons/api/data)

## driver

The driver used by this asset's container.

```
$asset->driver(); // Returns a string, eg. 'local', 's3'
```

## disk

Get the container's filesystem disk instance allowing you to chain file accessor methods.

```
$asset->disk(); // Returns FileAccessor
```

## folder

Get the folder this asset is located in.

```
$asset->folder(); // Returns AssetFolder
```

## basename

Get the basename.

```
$asset->basename(); // Returns a string, eg. photo.jpg
```

## filename

Get the filename (the basename without extension).

```
$asset->filename(); // Returns a string, eg. photo
```

## path

Get or set the path to the asset, relative to the asset container.

```
$asset->path(); // Returns a string, eg. img/photo.jpg
```
```
$asset->path($path); // Returns null
```

## resolvedPath

Get the resolved path to the asset. This is the path combined with the asset container's path.

```
$asset->resolvedPath(); // Returns a string, eg. assets/img/photo.jpg
```

## url

Get the URL of the asset.

This will concatenate the container's `url` and the path of the asset.

For a local driver, if the container url is relative, the asset URL will be, too.

For an Amazon S3 driver, the URL will absolute.

```
$asset->url(); // Returns a string, eg. /assets/img/photo.jpg
```

## uri

Alias of `url`.

## absoluteUrl

An absolute version of `url`.

```
$asset->absoluteUrl(); // Returns a string, eg. http://yoursite.com/assets/img/photo.jpg
```

## manipulate

Manipulate an image asset with Glide.

If you pass parameters, your Glide URL will be generated right away.

If you omit parameters, you will get an instance of `UrlBuilder` to continue chaining and create your URL.

```
$asset->manipulate(); // Returns UrlBuilder
```
```
$asset->manipulate($params); // Returns a string, eg. /img/id/123?w=100&h=100
```

## isImage

Check whether the asset is an image. (`jpg`, `jpeg`, `png`, or `gif`)

```
$asset->isImage(); // Returns a boolean
```

## extension

Get the file extension of the asset.

```
$asset->extension(); // Returns a string, eg. 'jpg'
```

## lastModified

Get the date the asset was last modified, as a `Carbon` instance.

```
$asset->lastModified(); // Returns Carbon
```

## container

Get the asset container this asset belongs to.

```
$asset->container(); // Returns AssetContainer
```

## dimensions

Get the dimensions of the asset, if it's an image.

```
$photo->dimensions(); // Returns an array, eg. [200, 150]
```
```
$file->dimensions(); // Returns an array, eg. [null, null]
```

## width

Get the asset's width.

```
$asset->width(); // Returns an integer
```

## height

Get the asset's height.

```
$asset->height(); // Returns an integer
```

## upload

Upload a file. Accepts an instance of `Symfony\Component\HttpFoundation\File\UploadedFile`.

```
$asset->upload(UploadedFile $file); // Returns null
```
