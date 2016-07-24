---
title: Collection
class: Statamic\API\Collection
id: 3839bae6-87a2-4936-879e-67872e953a08
---
A collection in this context is a container of entries.

Not to be confused with `Illuminate\Support\Collection`. Although that's awesome, too. [Check it out](https://laravel.com/docs/5.1/collections).

## Get all collections.

``` php
Collection::all(); // Returns \Illuminate\Support\Collection
```

## Get the handles of all collections.

``` php
Collection::handles(); // Returns an array of strings
```

## Get a collection by handle.

``` php
Collection::whereHandle($handle); // Returns Collection
```

## Check if a collection exists.

``` php
Collection::handleExists($handle); // Returns a boolean
```

## Create a collection.

``` php
Collection::create($handle); // Returns Collection
```
