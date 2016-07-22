---
id: 9954b574-ea65-4c1a-a9ec-193f9fa4b23e
class: Statamic\API\Asset
title: Asset
---
## Get all Assets.

``` php
Asset::all(); // Returns AssetCollection
```

## Get assets in a container.

``` php
Asset::whereContainer($container); // Returns AssetCollection
```

## Get assets in a folder (and container).

``` php
Asset::whereFolder($folder, $container); // Returns AssetCollection
```

## Get an asset by ID.

``` php
Asset::find($id); // Returns Asset
```

## Get an asset by path.

``` php
Asset::wherePath($path); // Returns Asset
```

## Check if an asset exists.

``` php
Asset::exists($id); // Returns a boolean
```

## Create an asset.

This returns an instance of a `AssetFactory` to allow you to chain and build your asset.

```
Asset::create(); // Returns AssetFactory
```
