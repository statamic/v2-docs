---
title: GlobalSet
class: Statamic\API\GlobalSet
id: a53ff28d-8421-4bde-bfdf-7288942cc6d3
---
We'd call this class `Global` if PHP was cool with it. But it isn't.

## Get all global sets.

``` php
GlobalSet::all(); // Returns GlobalCollection
```

## Get a global by ID.

``` php
GlobalSet::find($id); // Returns GlobalSet
```

## Get a global by handle (aka: slug).

``` php
GlobalSet::whereHandle($handle); // Returns GlobalSet
```

## Create a global.

This returns an instance of a `GlobalFactory` to allow you to chain and build your global.

```
GlobalSet::create($handle); // Returns GlobalFactory
```
