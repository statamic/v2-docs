---
title: Batch
overview: Convert a batch of img URLs to their Glide counterparts.
parameters:
  - 
    name: glide parameters
    type: mixed
    description: |
      All of the API manipulation parameters listed on the [Glide tag][tag].
      [tag]: /tags/glide#parameters
id: 5173c6fb-8c28-4cb1-9d2e-b7c902f96308
---
## Usage
This is a tag pair that wraps content containing `<img />` tags. Each image tag's `src` attribute will be converted to Glide URLs.

A common use case for this tag would be to resize all raw image assets inserted into a Markdown or Redactor field.

## Example

We have a markdown field with some images sprinkled througout.

``` language-yaml
description: |
  Bears
  ![](/assets/bears.jpg)
  Beets
  ![](/assets/beets.jpg)
  Battlestar Galactica
```

```
{{ glide:batch width="300" height="200" fit="crop" filter="sepia" }}
  {{ description | markdown }}
{{ /glide:batch }}
```

``` .language-output
<p>Bears
<img src="/img/assets/bears.jpg?w=300&h=200&fit=crop&filt=sepia" />
Beets
<img src="/img/assets/beets.jpg?w=300&h=200&fit=crop&filt=sepia" />
Battlestar Galactica</p>
```

[glide_tag]: /tags/glide
