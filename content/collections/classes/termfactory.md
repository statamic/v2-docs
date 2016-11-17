---
title: TermFactory
class: Statamic\Data\Taxonomies\TermFactory
id: fa4bfc78-cc0d-4bc1-888a-3ae5d6181954
---
Inheritance: [ContentFactory](/addons/api/contentfactory)

## Usage

Create a `TermFactory` using the API class. You should pass the slug into the `create` method.

```
$factory = Statamic\API\Term::create('my-term');
```

## Available Methods

### taxonomy

Specify the taxonomy, as a string.

```
$factory->taxonomy('tags');
```
