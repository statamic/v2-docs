---
title: Cache
overview: >
  There are times when you may want to simply cache a section of a template to gain some performance. That’s what this Tag is for. Wrap your markup and you’re off on your way to a snappier, zippier, peppier site.
parameters:
  -
    name: scope
    type: 'string *site*'
    description: 'Sets the cache scope. You have your choice of the default `site` or `page`.'
id: 1d0d2d1f-734b-4360-af7a-6792bf670bc7
---
## Usage {#usage}

The content between the `{{ cache }}` and `{{ /cache }}` tags is hashed to create a unique key for that tag. The cacher then checks to see if a cache for that unique key already exists. If it exists it displays the rendered HTML, otherwise it will parse and render the content, store that HTML for use later, and _then_ display everything knowing full well it won't have to do that work again next time.

```
{{ cache }}
  {{ collection:stocks limit="5000" }}
    <!-- probably lots of things happening -->
  {{ /collection:stocks }}
{{ /cache }}
```

Cache is smart. Cache doesn't do more work than it has to. We should all take a lesson from Cache's autobiography (entitled _"Catch My Cash Cache If You Can"_).
