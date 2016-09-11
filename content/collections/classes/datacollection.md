---
title: DataCollection
class: Statamic\Data\DataCollection
id: ec607df4-2cfb-43b3-ae96-2b07d95804f4
---
Inheritance: [Illuminate\Support\Collection](https://laravel.com/docs/5.1/collections)

## Usage

A `DataCollection` is used to hold a collection of `Data` objects. This could be a combination of `Page`, `Entry`, `Asset`, `User`, etc.

The examples on this page use simple values to make things easier to understand.

## Creating a Collection

You can create a DataCollection in a similar fashion to a regular collection.

```
$collection = collect_data([1, 2, 3]);
```

## Available Methods

Just like regular collections, the following methods can be chained for fluent manipulation of the underlying array.

### Limit {#limit}

Limit the collection by the specified number of items. An alias of `$this->take()`.

```
$collection = collect([0, 1, 2, 3, 4, 5]);

$chunk = $collection->limit(3);

$chunk->all();

// [0, 1, 2]
```

### Multisort

Sort a collection by multiple fields.

Accepts a string like `title:desc|foo:asc`. The keys are optional. `title:desc|foo` is fine.

```
$collection = collect([
    ['name' => 'Desk', 'price' => 200],
    ['name' => 'Desk', 'price' => 100],
    ['name' => 'Bookcase', 'price' => 150],
]);

$sorted = $collection->multisort('name|price');

$sorted->values()->all();

/*
    [
        ['name' => 'Bookcase', 'price' => 150],
        ['name' => 'Desk', 'price' => 100],
        ['name' => 'Desk', 'price' => 200],
    ]
*/
```

### Actions

Walk over an array of methods and attempt to run each one.

```
$collection = collect_data(['foo' => 'Foo', 'bar' => 'Bar', 'baz' => 'Baz']);

$collection = $collection->actions([
    'limit' => 2,
    'flip' => true
]);

$collection->all();

/*
    [
        'Foo' => 'foo'
        'Bar' => 'bar'
    ]
*/
```

### Conditions

Filter the collection using [the conditions syntax](/conditions).

```
// Remember, in reality these would be Data objects.
$collection = collect_data([
    ['meat' => 'Bacon'],
    ['meat' => 'Beef'],
    ['meat' => 'Chicken']
]);

// Translated from a template tag with `meat:contains="a"`
$collection = $collection->conditions(['meat:contains' => 'a']);

/*
    [
        ['meat' => 'Bacon']
    ]
*/
```

### Supplement

Supplement each item with a value. Supplemental data on objects is added to the resulting array when using `->toArray()`.

```
// Remember, in reality these would be Data objects
$collection = collect_data([
    ['title' => 'One']
    ['title' => 'Two']
]);

$collection = $collection->supplement('foo', function ($item) {
    return strrev($item->get('title'));
});

$collection->toArray();

/*
    [
        ['title' => 'One', 'foo' => 'enO'],
        ['title' => 'Two', 'foo' => 'owT']
    ]
*/
```

### To Array

Changing to an array works the same as the parent class except keys are removed.

```
// Remember, in reality these would be Data objects
$collection = collect_data([
    'one' => ['title' => 'Foo'],
    'two' => ['title' => 'Bar']
]);

$collection->toArray();

/*
    [
        ['title' => 'Foo'],
        ['title' => 'Bar']
    ]
*/
```

### To Array With

Convert to an array with only a subset of keys.

```
// Remember, in reality these would be Data objects
$collection = collect_data([
    'one' => ['title' => 'Foo', 'size' => 'large'],
    'two' => ['title' => 'Bar', 'size' => 'small']
]);

$collection->toArrayWith(['title']);

/*
    [
        ['title' => 'Foo'],
        ['title' => 'Bar']
    ]
*/
```
