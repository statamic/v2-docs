---
class: Statamic\API\AssetContainer
title: AssetContainer
id: 7ac04912-e3c2-47ac-9536-b31118f8aeb2
---
An asset container is the "source" where a set of assets and folders are located.

Not to be confused with `AssetCollection`, which is a subclass of `Illuminate\Support\Collection`, and holds a collection of `Asset` objects.

## Get all Asset Containers.

``` php
AssetContainer::all(); // Returns \Illuminate\Support\Collection
```

## Get an asset container by ID.

``` php
AssetContainer::find($id); // Returns AssetContainer
```

## Get an asset container by path.

``` php
AssetContainer::wherePath($path); // Returns AssetContainer
```

## Create an asset container.

This returns an instance of a `AssetContainerFactory` to allow you to chain and build your asset container.

```
AssetContainer::create(); // Returns AssetContainerFactory
```
