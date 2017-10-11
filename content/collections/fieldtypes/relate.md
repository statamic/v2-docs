---
title: Relate
description: Create relationships with other content.
overview: "Allows you create relationships. Data relationships that is, this doesn't help your online dating game much, if at all."
image: /assets/fieldtypes/relate.png
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
  -
    name: mode
    type: string *tags*
    description: "Available UI modes are `panes` and `tags`."
---
## Relationship Types {#relationship-types}

You can relate any of the base [Content Types](/content-types), each of which with its own specific fieldtype. They all extend this core Relate fieldtype.

- [Taxonomy](/fieldtypes/taxonomy) - for Taxonomy Terms
- [Collection](/fieldtypes/collection) - for Entries
- [Pages](/fieldtypes/pages) - for Pages
- [Users](/fieldtypes/users) - for Users
- [Assets](/fieldtypes/assets) - for Assets (it doesn't extend this Relate fieldtype)
