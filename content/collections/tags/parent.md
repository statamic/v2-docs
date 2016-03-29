---
title: Parent
overview: Grab the current page's parent's page data. You follow that?
parameters:
  -
    name: field
    type: tag part
    description: |
      The name of the field. This is not actually a parameter, but part of the tag itself.
      For example, `{{ parent:title }}`
id: 932ae2b5-0ff0-40e3-b8a4-1c71784917e4
---
## Example {#example}

``` .language-yaml
# pages/about/index.md
title: About
```

``` .language-yaml
# pages/about/team/index.md
title: Team
```

If you were on `/about/team` and use the following template:

```
{{ title }}
{{ parent:title }}
```

You would get this:

``` .language-output
Team
About
```
