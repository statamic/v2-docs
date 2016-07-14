---
title: Page
class: Statamic\API\Page
id: f871b95e-3861-43d2-957b-9002601eb52b
---
## Get all pages.

``` php
Page::all(); // Returns PageCollection
```

## Get a page by ID.

``` php
Page::find($id); // Returns Page
```

## Get a page by URI.

``` php
Page::whereUri($uri); // Returns Page
```

## Check if a page exists.

``` php
Page::exists($id); // Returns a boolean
```

## Check if a page exists by URI.

``` php
Page::uriExists($uri); // Returns a boolean
```

## Create a page.

This returns an instance of a `PageFactory` to allow you to chain and build your page.

```
Page::create($uri); // Returns PageFactory
```
