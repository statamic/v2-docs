---
title: Relate
description: Create relationships with other content.
overview: "Allows you create relationships. Data relationships that is, this doesn't help your online dating game much, if at all."
image: 0c490534-470b-4255-a19a-29739fef10cf
id: 9dd58c40-6e33-49c8-83fa-61a69f6371be
options:
  -
    name: max_items
    type: integer
    description: >
      The maximum number of items that may be selected. Setting this to `1` will change the UI to a dropdown.
  -
    name: sort
    type: string
    description: >
      Sort the suggestions by `fieldname:order`. You can add additional rules separated by pipes.
      Eg: `date:desc|title:asc`
  -
    name: label
    type: string
    description: >
      How the values should appear. You may use variables within the string, eg. `"{{ title }} ({{ date format="Y" }})"`
---
## Relationship Types

You can relate any of the base [Content Types](/guides/content-types), each of which with its own specific fieldtype. They all extend this core Relate fieldtype.

- [Taxonomy](/fieldtypes/taxonomy) - for Taxonomy Terms
- [Collection](/fieldtypes/collection) - for Entries
- [Users](/fieldtypes/users) - for Users
- [Assets](/fieldtypes/assets) - for Assets (it doesn't extend this Relate fieldtype)

## Data Structure {#data-structure}

Content relationships are formed between files through unique IDs. This `id` prevents any disruptions if their slugs, urls, or any other identifying fields are modified.

``` .language-yaml
wildlife:
  - 892jfsd9a90as
  - 134jk1h78dfas
```

### Templating {#templating}

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
