---
title: PageFolder
class: Statamic\API\PageFolder
id: e0203629-e729-4c75-905a-23fc8bedf735
overview: A page folder corresponds to the `folder.yaml` files located within the page tree.
---

## Get all page folders.

``` php
PageFolder::all(); // Returns \Illuminate\Support\Collection
```

A `PageFolder` will only exist when there is a `folder.yaml`. There isn't a `PageFolder` for every _folder_.


## Get a page folder by its handle (path).

A page folder's handle is also it's path, which would be something like `pages/1.about/folder.yaml`.

``` php
PageFolder::whereHandle($handle); // Returns PageFolder
```

## Check if a page folder exists by its handle.

``` php
PageFolder::handleExists($handle); // Returns a boolean
```

## Create a page folder

``` php
PageFolder::create($handle); // Returns a PageFolder
```
