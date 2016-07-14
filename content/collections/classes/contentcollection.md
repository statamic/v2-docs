---
title: ContentCollection
class: Statamic\Data\Content\ContentCollection
id: abc3c15e-21f4-425f-b6e9-8c511e3ce479
---
Inheritance: [DataCollection][datacollection] > [Illuminate\Support\Collection](https://laravel.com/docs/5.1/collections)

## Usage

A `ContentCollection` is used to hold a collection of `Content` objects. This could be a combination of `Page`, `Entry`, `Term`, or `GlobalContent` objects.

The examples on this page use simple values to make things easier to understand.

## Creating a Collection

You can create a `ContentCollection` in a similar fashion to a [DataCollection][datacollection] or a regular collection.

```
$collection = collect_content([1, 2, 3]);
```

## Available Methods

Just like regular collections, the following methods can be chained for fluent manipulation of the underlying array.

### Localize

Swap out the locale of each of the underlying objects.

```
$page->locale(); // en

$collection = collect_content([$page])->localize('fr');

$collection->first()->locale(); // fr
```

If the argument passed to the `localize` method starts with `only `, it will _also_ remove any items that don't already have data in the requested locale.

```
$page1->locales(); // ['en', 'fr']
$page2->locales(); // ['en']
$page3->locale();  // ['en', 'fr']

$collection = collect_content([$page1, $page2])->localize('only fr');

$collection->all(); // [$page1, $page3]
```

### Entries

Removes any items that aren't entries.

```
$collection = collect_content([$page1, $page2, $entry1, $entry2]);

$collection->entries()->all(); // [$entry1, $entry2]
```

### Pages

Removes any items that aren't pages.

```
$collection = collect_content([$page1, $page2, $entry1, $entry2]);

$collection->pages()->all(); // [$page1, $page2]
```

### Remove Unpublished

Removes any unpublished items (aka drafts).

```
$collection = collect_content([$draft1, $draft2, $published]);

$collection->removeUnpublished()->all(); // [$published]
```

### Filter by Taxonomy Term

Removes any items that don't belong to the given taxonomy term ID.

``` .language-yaml
# entry1.md
---
title: Entry One
tags: [123, 456]
---
```

``` .language-yaml
# entry2.md
---
title: Entry Two
tags: [456, 789]
---
```

```
$collection = collect_content([$entry1, $entry2]);

$collection->filterByTaxonomy(123)->all(); // [$entry1]
```



[datacollection]: /addons/classes/datacollection
