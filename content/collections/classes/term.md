---
class: Statamic\Data\Taxonomies\Term
title: Term
id: 1ec6a8eb-3e90-4302-8cc2-5048dd663e6d
---
Inheritance: [Content](/addons/api/content) > [Data](/addons/api/data)

## taxonomy

Get or set the associated taxonomy. You can pass either a string or a `Taxonomy`.

```
$term->taxonomy(); // Returns Taxonomy
```
```
$term->taxonomy($taxonomy); // Returns null
```

## taxonomyName

Get or set the associated taxonomy.

```
$term->taxonomy(); // Returns a string, eg. 'tags'
```
```
$term->taxonomy('tags'); // Returns null
```

## count

Get the number of content objects that use this term.

```
$term->count(); // Returns an integer
```
