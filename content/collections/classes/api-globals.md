---
title: Globals
class: Statamic\API\Globals
id: a53ff28d-8421-4bde-bfdf-7288942cc6d3
---
One _Global_ actually contains multiple variables. When you have one `GlobalContent` class, this can be thought of as one _file_.

## Get all globals.

``` php
Globals::all(); // Returns GlobalCollection
```

## Get a global by ID.

``` php
Globals::find($id); // Returns GlobalContent
```

## Get a global by handle (aka: slug).

``` php
Globals::whereHandle($handle); // Returns GlobalContent
```

## Create a global.

This returns an instance of a `GlobalFactory` to allow you to chain and build your global.

```
Globals::create($handle); // Returns GlobalFactory
```
