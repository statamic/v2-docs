---
title: Pages
overview: Allows you attach other Pages to your content.
image: 1ae0abcc-d9c7-412c-8647-fbeedaaa8444
options:
  -
    name: parent
    type: string
    description: The URL or ID of a page if you want to only show its children.
  -
    name: depth
    type: integer
    description: The level of child pages to traverse when using the `parent` option.
extends: 9dd58c40-6e33-49c8-83fa-61a69f6371be
id: db75755f-f1b8-4281-8995-2723dd92d967
---
## Data Structure {#data-structure}

The Pages fieldtype is a [Relate fieldtype](/fieldtypes/relate), which means the Pages will be saved as IDs.

``` .language-yaml
pages:
  - 892jfsd9a90as
  - 134jk1h78dfas
```

## Templating {#templating}

Use the [Relate tag](/tags/relate) to loop through the IDs and fetch the content data.

```
<ul>
  {{ relate:pages }}
    <li>{{ title }}</li>
  {{ /relate:pages }}
</ul>
```

``` .language-output
<ul>
  <li>How to Basic</li>
  <li>How to Extreme</li>
</ul>
```
