---
title: Fieldtypes
template: section
id: ef235bbf-9f7f-4ce0-9c81-bd2295dbe6a6
mount: fieldtypes
overview: >
  Fieldtypes give you the ability to structure your content through the use of tailored interfaces designed for different types of data. They are the Control Panel connection to the Flat Files that make Statamic what it is. Every fieldtype is ultimately writing YAML in a pre-determined format for you.
options:
  -
    name: display
    type: string
    description: The label shown above the field
  -
    name: instructions
    type: string
    description: Help text shown underneath the display label. Markdown supported.
  -
    name: localizable
    type: boolean
    description: Enable localization (the ability to translate this field)
  -
    name: width
    type: int
    description: The fields's width layout, in percentages (e.g. `100`, `50`)
  -
    name: validate
    type: string
    description: 'A pipe delimited string of [validation rules](http://laravel.com/docs/5.1/validation#available-validation-rules)*'
  -
    name: bold
    type: boolean *false*
    description: Setting to true will make the field label bold.
  -
    name: show_when
    type: string|array
    description: |
      The conditions under which this field should be displayed. You can do things like "show this field when this other field has this value"*.  
      [Learn how to configure conditional fields](/fieldsets#conditional-fields)
  -
    name: hide_when
    type: string|array
    description: 'The same as `show_when`, but with the logic inverted*.'
  -
    name: replicator_preview
    type: boolean *true*
    description: |
      When set to `false`, this field will not be displayed in the preview text for a collapsed [Replicator][replicator] set.
      [replicator]: /fieldtypes/replicator

---
## Default Settings {#default-settings}

Most fieldtypes share a common set of default settings, like validation, display text, instructions, and so on. You'll see this list referenced throughout the docs.

* Not available on fields inside a bard, replicator or grid field.
