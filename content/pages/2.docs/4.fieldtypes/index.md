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
    description: The fields's width layout, in percentages (e.g., `100`, `75`, `66`, `50`, `33`, or `25`; other values are ignored. )
  -
    name: validate
    type: string
    description: 'A pipe delimited string of [validation rules](http://laravel.com/docs/5.1/validation#available-validation-rules)'

---
## Default Settings {#default-settings}

Most fieldtypes share a common set of default settings, like validation, display text, instructions, and so on. You'll see them referenced throughout the docs, so we've detailed them here for you.
