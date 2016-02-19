---
title: Pages
is_parent_tag: true
overview: Iterate over child pages.
parameters:
  -
    name: url
    type: string
    description: >
      The URL of the page from where to find
      the pages. If this parameter isn’t
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
  -
    name: depth
    type: integer *1*
    description: How deep to traverse the hierarchy to find child pages.
id: 73f02fa0-d60a-47f6-b5c2-226fcb3d4d8b
---
## Usage {#usage}

This tag has the same functionality as the [collection](collection) tag, with some differences.

The `pages` tag will attempt to get the child pages of a specified parent page.

For example, given this page structure:

``` .language-files
about/
|-- index.md
|-- 1.team/
|   |-- 1.board/
|   |   |-- index.md
|   |-- index.md
|-- 2.news/
|   |-- index.md
```

```
{{ pages from="/about" }}
   {{ order }}. {{ title }}
{{ /pages }}
```

``` .language-output
1. Team
2. News
```

Notice only the immediate child pages are output. If we wanted to get the next level, we can specify the `depth` parameter.

```
{{ pages from="/about" depth="2" }}
   {{ order }}. {{ title }}
{{ /pages }}
```

``` .language-output
1. Board
1. Team
2. News
```

Notice that (by default) the pages are being sorted by the order key, then alphabetically. They are also displayed in a
single list, as siblings. If you'd like to output the hierarchy, you should use the [Nav Tag](/docs/tags/nav).

[collection]: /docs/tags/collection
