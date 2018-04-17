---
title: EntryFactory
class: Statamic\Data\Entries\EntryFactory
id: bd8de4ec-bbf5-4095-8673-8d9343ee67ae
---
Inheritance: [ContentFactory](/addons/api/contentfactory)

## Usage

Create a `EntryFactory` using the API class. You should pass the slug into the `create` method.

```
$factory = Statamic\API\Entry::create('my-post');
```

## Available Methods

### collection

Specify the collection, as a string.

```
$factory->collection('blog');
```

### date

An alias for setting the order as a date string, with the benefit of using Carbon to parse the provided string. Not providing an argument will use the current date.

```
$factory->date('2016-01-01');

$factory->date('1 week ago');

$factory->date(); // now
```
