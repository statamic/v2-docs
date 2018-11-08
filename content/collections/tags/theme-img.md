---
title: Image
overview: Get the URL to an image in your theme.
parameters:
  -
    name: src
    type: string
    description: >
      The path to the image, relative to the
      theme/img directory.
  -
    name: cache_bust
    type: 'boolean *false*'
    description: >
      Setting this to `true` will add the timestamp of the asset to the end of
      the URL in a `?v=` query param.
  -
    name: absolute
    type: boolean *false*
    description: Render the URL in an absolute format.
---
## Example {#example}
```
{{ theme:img src="hat.jpg" }}
```
``` .language-output
/site/themes/redwood/img/hat.jpg
```

Add the `tag` parameter to output a `img` tag.

```
{{ theme:img src="hat.jpg" tag="true" }}
```
``` .language-output
<img src="/site/themes/redwood/img/hat.jpg">
```