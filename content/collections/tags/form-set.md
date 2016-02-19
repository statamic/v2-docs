---
title: Formset
id: 0b9590a7-f8b3-4a11-92b5-60d6d43cf869
overview: >
  A shortcut tag to allow all child Form tags to inherit the formset.
parameters:
  -
    name: in|is|formset
    type: string
    description: >
      The name of the formset to use. You can use `in`, `is`, or `formset`. Whichever feels more natural to you.
---
## Usage {#usage}

All the `Form` tags need to know the formset they are handling. Rather than specifying the formset parameter
on each tag, we can use the `{{ form:set }}` tag to automatically apply them.

```
{{ form:set is="contact" }}

    {{ if {form:errors} }}
      {{ form:errors }}...{{ /form:errors }}
    {{ /if }}

    {{ if {form:success} }}...{{ /if }}

    {{ form:create }}...{{ /form:create }}

{{ /form:set }}
```

In this example, if we didn't use the `form:set` wrapper tag, we would need to add `in="contact"` to each of the
`form:something` tags.
