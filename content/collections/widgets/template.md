---
title: Template
overview: Display anything you want using a standard Antlers template.
description: Display anything you want using a standard Antlers template from your theme.
parameters:
  -
    name: template
    type: string
    description: |
      The template to render. It should be relative to your theme's template directory.
      eg. `bacon` would correspond to `site/themes/yourtheme/templates/bacon.html`.
id: 6e884f39-63ad-46e0-9ce9-703463fccedf
---
## Overview

You can use the Template Widget as a completely customizable block of HTML rendered by [Antlers](/antlers).

Any additional parameters you specify in the widget config will be available as variables in your template.

## Example

``` .language-yaml
widgets:
  -
    type: widget
    template: bacon
    cooking_method: fried
```

```
I like my bacon {{ cooking_method }}!
```

``` .language-output
I like my bacon fried!
```
