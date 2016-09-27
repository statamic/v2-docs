---
title: Taxonomy
extends: 9dd58c40-6e33-49c8-83fa-61a69f6371be
description: Attach Taxonomy Terms to your content.
overview: >
  Allows you attach Taxonomy Terms to your content. Tagging, Categories, Colors, Flavors, you name it. Taxonomy all the things!
image: 0c490534-470b-4255-a19a-29739fef10cf
options:
  -
    name: taxonomy
    type: string
    description: > The handle of the Taxonomy from which to fetch Terms.
  -
    name: mode
    type: string *tags*
    description: "Available UI modes are `panes` and `tags`."
  -
    name: label
    type: string
    description: >
      You can choose to display the terms using custom field data. Example:   `label: "{{ slug }} ({{ description }})"`
id: 31adcc00-4fbb-4fe9-9b48-401061273096
---
## Data Structure

The Taxonomy fieldtype is a [Relate fieldtype](/fieldtypes/relate), which means the taxonomies will be saved as IDs.

``` .language-yaml
wildlife:
  - 892jfsd9a90as
  - 134jk1h78dfas
```

## Templating

Use the [Relate tag](/tags/relate) to loop through the IDs and fetch the content data.

```
<ul>
  {{ relate:wildlife }}
    <li>{{ title }}</li>
  {{ /relate:wildlife }}
</ul>
```

``` .language-output
<ul>
  <li>Moose</li>
  <li>Squirrel</li>
</ul>
```
