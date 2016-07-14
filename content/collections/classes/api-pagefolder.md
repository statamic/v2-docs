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
