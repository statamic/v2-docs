---
title: ContentFactory
class: Statamic\Data\Content\ContentFactory
id: 5de91d0c-484a-4ac2-b12e-5d140af4fa8e
overview: |
  A ContentFactory an abstract class is used to generate new content objects for you. These include `Page`, `Entry`, `GlobalContent`, and `Term`.
---

## Usage

You can instantiate a factory by using the `create()` method in the corresponding data type's API class.

To create a `Page`, use `Statamic\API\Page::create()`.  
For an `Entry`, use `Statamic\API\Entry::create()`, and so on.

Once you have a factory instance, you can chain the methods on the factory.

## Available Methods

The following methods are available on this class (and therefore on subclasses).

### order

The order of the content. For instance, pages can have a number. Entries can have a number, date, or no order, depending on the type of collection they're in.

```
$factory->order(2);
```

### path

Data objects will be able to determine their paths automatically under most conditions. Use this to specify it manually.

```
$factory->path('pages/1.about/index.md');
```

### published

Whether this should be published. Accepts a boolean.

```
$factory->published(true);
```

### with

Add an array of data to the object. "Create an object _with_ this data."

```
$factory->with(['foo' => 'bar', 'baz' => 'qux']);
```
