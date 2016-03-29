---
title: Relate
description: Create relationships with other content.
overview: "Allows you create relationships. Data relationships that is, this doesn't help your online dating game much, if at all."
image: 0c490534-470b-4255-a19a-29739fef10cf
id: 9dd58c40-6e33-49c8-83fa-61a69f6371be
---
## Relationship Types

You can relate any of the base [Content Types](/guides/content-types), each of which with its own specific fieldtype. They all extend this core Relate fieldtype.

- [Taxonomy](/docs/fieldtypes/taxonomy) - for Taxonomy Terms
- [Collection](/docs/fieldtypes/collection) - for Entries
- [Users](/docs/fieldtypes/users) - for Users
- [Assets](/docs/fieldtypes/assets) - for Assets (it doesn't extend this Relate fieldtype)

## Data Structure {#data-structure}

Content relationships are formed between files through unique IDs. This `id` prevents any disruptions if their slugs, urls, or any other identifying fields are modified.

``` .language-yaml
wildlife:
  - 892jfsd9a90as
  - 134jk1h78dfas
```

### Templating {#templating}

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
