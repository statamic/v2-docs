---
title: Content
class: Statamic\API\Content
id: 064277b2-9712-43f4-b4f0-b75a6affef3f
---
This class is used for accessing “content”. You’d normally use this if you have to deal with IDs/URIs and you don’t necessarily know if they belong to a particular content type (Pages, Entries, Terms).

If you know you will be dealing with a specific content type, you should use one of the appropriate classes. `Statamic\API\Page`, or `Statamic\API\Entry`, for example.

## Get all content

``` php
Content::all(); // Returns ContentCollection
```

## Get content by ID

``` php
Content::find($id); // Returns Content
```

## Get content by URI

``` php
Content::whereUri($uri); // Returns Content
```

## Check if content exists.

``` php
Content::exists($id); // Returns boolean
```

## Check if content exists by URI.

``` php
Content::uriExists($uri); // Returns boolean
```

## Get the content structure tree.

``` php
Content::tree($uri = null, $depth = null, $entries = null, $drafts = null, $exclude = null, $locale = null)
```
