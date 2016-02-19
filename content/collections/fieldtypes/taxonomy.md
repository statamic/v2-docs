---
title: Taxonomy
overview: >
  Allows you attach Taxonomy Terms to your
  content. Tagging, Categories, Colors, Flavors, you name it. Taxonomy all the things!
image: 0c490534-470b-4255-a19a-29739fef10cf
options:
  -
    name: taxonomy
    type: string
    description: >
      The name of the Taxonomy from which to
      fetch Terms.
id: 31adcc00-4fbb-4fe9-9b48-401061273096
---
## Data Structure

The Taxonomy fieldtype is a [Relate fieldtype](/docs/fieldtypes/relate), which means the taxonomies will be saved as IDs.

``` .language-yaml
wildlife:
  - 892jfsd9a90as
  - 134jk1h78dfas
```

## Templating

Use the [Relate tag](/docs/tags/relate) to loop through the IDs and fetch the content data.

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
