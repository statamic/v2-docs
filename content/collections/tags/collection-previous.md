---
title: Previous
overview: >
  Iterate over the previous entries that
  would show up in the listing.
id: 741cf972-c0bd-4e3c-81e2-8cc8bea60737
---
This Tag functions the same way as the [`collection:next`](/docs/tags/collection-next) tag, but in the opposite direction.

```
{{ collection:previous in="blog" as="posts" limit="2" sort="date:asc" }}

  {{ if no_results }}
    No more posts to read!
  {{ /if }}

  {{ posts }}
    <div class="post">
      <a href="{{ url }}">{{ title }}</a>
    </div>
  {{ /posts }}

{{ /collection:previous }}
```
