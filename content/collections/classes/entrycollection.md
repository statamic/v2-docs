---
title: EntryCollection
class: Statamic\Data\Entries\EntryCollection
id: 5796a6e9-8ae5-4759-a962-10987ec552fb
---
Inheritance: [ContentCollection][contentcollection] > [DataCollection][datacollection] > [Illuminate\Support\Collection](https://laravel.com/docs/5.1/collections)

## Usage

An `EntryCollection` is used to hold a collection of `Entry` objects.

This is not to be confused with a `CollectionFolder`, which is like what a `Taxonomy` is to a `Term`.

The examples on this page use simple values to make things easier to understand.

## Creating a Collection

You can create an `EntryCollection` in a similar fashion to a [ContentCollection][contentcollection] or a regular collection.

```
$collection = collect_entries([1, 2, 3]);
```

## Available Methods

Just like regular collections, the following methods can be chained for fluent manipulation of the underlying array.

### Remove Future

Remove entries with a date in the future.

```
$collection = collect([$past1, $past2, $future1]);

$collection->removeFuture()->all(); // [$past1, $past2]
```

### Remove Past

Remove entries with a date in the past.

```
$collection = collect([$past1, $past2, $future1]);

$collection->removePast()->all(); // [$future1]
```

### Remove Before

Remove entries with a date before a given date. The given date will be parsed by Carbon.

```
$collection = collect([$before1, $before2, $after1]);

$collection->removeBefore('-1 week')->all(); // [$after1]
```

### Remove After

Remove entries with a date after a given date. The given date will be parsed by Carbon.

```
$collection = collect([$before1, $before2, $after1]);

$collection->removeAfter('+1 week')->all(); // [$before1, $before2]
```


[contentcollection]: /addons/classes/contentcollection
[datacollection]: /addons/classes/datacollection
