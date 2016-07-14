---
title: Entry
class: Statamic\API\Entry
id: 25722490-c95d-450c-b27d-dd9b40587f97
---
## Get all entries.

``` php
Entry::all(); // Returns EntryCollection
```

## Get all entries in a collection

``` php
Entry::whereCollection($collection); // Returns EntryCollection
```

## Get an entry by ID.

``` php
Entry::find($id); // Returns Entry
```

## Get an entry by slug (and collection).

``` php
Entry::whereSlug($slug, $collection); // Returns Entry
```

## Get an entry by URI

``` php
Entry::whereUri($uri); // Returns Entry
```

## Check if an entry exists.

``` php
Entry::exists($id); // Returns a boolean
```

## Check if an entry exists by slug.

``` php
Entry::slugExists($slug, $collection); // Returns a boolean
```

## Create an entry.

This returns an instance of a `EntryFactory` to allow you to chain and build your entry.

```
Entry::create($slug); // Returns EntryFactory
```
