---
class: Statamic\Data\Pages\Page
title: Page
id: 41d42ed3-628a-4f90-a7ef-e1b81e970808
---
Inheritance: [Content](/addons/classes/content) > [Data](/addons/classes/data)

## hasEntries

Whether this page has a collection of entries mounted to it.

```
$page->hasEntries(); // Returns a boolean
```

## entries

Get the entries mounted to the page.

```
$page->entries(); // Returns EntryCollection
```

## entriesCollection

Get the name of the entry collection mounted to the page.

```
$page->entriesCollection(); // Returns a string, eg. 'blog'
```

## children

Get this page's child pages. The first argument is the depth you wish to traverse. By default it will traverse as deep
as possible.

```
$page->children(); // Returns PageCollection
```

## parent

Get the parent page.

Top level pages treat the home page as their parent. The homepage doesn't have a parent.

```
$page->parent(); // Returns Page
```
```
$homepage->parent(); // Returns null
```
