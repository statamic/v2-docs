---
title: Term
class: Statamic\API\Term
id: 7c3b86e0-f993-4eb4-8e36-1486b8e27937
---
## Get all terms.

``` php
Term::all(); // Returns TermCollection
```

## Get a term by ID.

``` php
Term::find($id); // Returns Term
```

## Get a term by slug (and taxonomy).

``` php
Term::whereSlug($slug, $taxonomy); // Returns Term
```

## Get a term by URI.

``` php
Term::whereUri($uri); // Returns Term
```

## Get all terms in a taxonomy.

``` php
Term::whereTaxonomy($taxonomy); // Returns TermCollection
```

## Create a term.

This returns an instance of a `TermFactory` to allow you to chain and build your term.

```
Term::create($slug); // Returns TermFactory
```
