---
title: AssetCollection
class: Statamic\Data\Entries\AssetCollection
id: ea5e9f86-ebe4-498f-a92a-da3aa5bf6ea0
---
Inheritance: [DataCollection][datacollection] > [Illuminate\Support\Collection](https://laravel.com/docs/5.1/collections)

## Usage

An `AssetCollection` is used to hold a collection of `Asset` objects.

This is not to be confused with an `AssetContainer`, which is the "source" where assets and folders are located.

The examples on this page use simple values to make things easier to understand.

## Creating a Collection

You can create an `AssetCollection` in a similar fashion to a [DataCollection][datacollection] or a regular collection.

```
$collection = collect_assets([1, 2, 3]);
```

## Available Methods

Just like regular collections, the following methods can be chained for fluent manipulation of the underlying array.

### To Array

This is just like a [DataCollection][datacollection]'s `toArray` method, except if any assets do not have corresponding
files, they will be filtered out from the array.


[contentcollection]: /addons/classes/contentcollection
[datacollection]: /addons/classes/datacollection
