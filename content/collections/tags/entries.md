---
title: Entries
id: 45e7802d-2399-4a60-9cf8-35441efa9aab
is_parent_tag: true
overview: Iterate over entries in a Collection.
parameters:
  -
    name: url
    type: string
    description: >
      The URL of the page from where to find
      entries. If this parameter isn’t
      specified, Statamic will look at the
      current URL.
  -
    name: from
    type: string
    description: Alias of `url`
  -
    name: folder
    type: string
    description: Alias of `url`
---
## Usage {#usage}

This tag has the same functionality as the [collection](collection) tag, with some differences.

The `entries` tag will attempt to get the entries based on a mount point of a page.

For example:

```
{{ entries from="/company/news" }}
   ...
{{ /entries }}
```

In this tag, Statamic will check the `/company/news` page for any mounted entry collections. If it has a mounted collection (with `mount: articles`, for example) the tag will get the entries in the `articles` collection.

[collection]: /docs/tags/collection
