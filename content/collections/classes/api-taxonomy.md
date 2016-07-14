---
title: Taxonomy
class: Statamic\API\Taxonomy
id: b109a335-e262-4313-9f9a-1507ba31a22c
---
## Get all taxonomies.

``` php
Taxonomy::all(); // Returns \Illuminate\Support\Collection
```

## Get the handles of all taxonomies.

``` php
Taxonomy::handles(); // Returns an array of strings
```

## Get a taxonomy by handle.

``` php
Taxonomy::whereHandle($handle); // Returns TaxonomyFolder
```

## Check if a taxonomy exists.

``` php
Taxonomy::handleExists($handle); // Returns a boolean
```
