---
class: Statamic\Data\Entries\Entry
title: Entry
id: ed652d64-282d-40b7-9443-4ae72f8289b8
---
Inheritance: [Content](/addons/classes/content) > [Data](/addons/classes/data)

## collection

Get or set the associated collection. You can pass either a string or a `CollectionFolder`.

```
$entry->collection(); // Returns CollectionFolder
```
```
$entry->collection($collection); // Returns null
```

## collectionName

Get or set the associated collection.

```
$entry->collectionName(); // Returns a string, eg. 'blog'
```
```
$entry->collectionName('blog'); // Returns null
```

## orderType

Get the order type.

```
$entry->orderType(); // Returns a string, eg. 'date', 'number', 'alphabetical'
```

## date

Get the entry's date as a `Carbon` instance. If you try to call this on a non-date ordered entry, it will throw a
`InvalidEntryTypeException`.

```
$entry->date(); // Returns Carbon
```

## hasTime

Determine whether the entry has a timestamp.

```
$entry->hasTime(); // Returns a boolean
```
