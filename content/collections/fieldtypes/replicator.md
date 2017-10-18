---
title: Replicator
description: Build your content by creating sets of fields you can mix and match on the fly.
overview: |
  The Replicator is a meta fieldtype giving you the ability to define _sets_ of fields that you can dynamically piece together in whatever order and arrangement you imagine. You can build long-form articles like [Medium.com](http://medium.com) and take advantage of the extra markup control.

  It's so much better than a WYSIWYG field.
image: /assets/fieldtypes/replicator.png
options:
  -
    name: sets
    type: array
    description: An array containing sets of fields.
id: 00b140e3-413a-4d91-b9e7-65f58d56a41b
---
## Usage

You will be presented with a button for each set you’ve defined. Clicking one will replicate an empty set. You can [replicate](https://www.youtube.com/watch?v=qD4EVXkfe0w) a single set type as many times as you like as well as dragging and dropping them to adjust their order.

You may collapse your sets to conserve space. If you do, a preview of the data contained within it will be displayed. [Third party fieldtypes may control how their data will be previewed](/addons/classes/fieldtypes#replicator-preview-text).

## Data Structure {#data-structure}

Replicator stores your data as an array with the set name as `type`.

```.language-yaml
replicator:
  -
    type: quote
    quote: Where for art thou, Romeo?
    author: Juliet Capulet
  -
    type: image
    filename: map-to-romeo.jpg
    caption: "Romeo's Location"
```

> Please note that you **can not** use a Replicator fieldtype for the `content` field. Since the data is saved as an array,
it needs to be part of the front-matter.

## Templating {#templating}

Use the tag pair syntax with an `if` `else` conditions to style accordingly each set accordingly.

```
{{ my_replicator_field }}

  {{ if type == "content_set" }}

    <div class="text">
      {{ text|markdown }}
    </div>

  {{ elseif type == "quote_set" }}

    <setquote>{{ quote }}</setquote>
    <p>— {{ cite }}</p>

  {{ elseif type == "image_set" }}

    <figure>
      <img src="{{ photo }}" alt="{{ caption }}" />
      <figcaption>{{ caption }}</figcaption>
    </figure>

  {{ /if }}

{{ /my_replicator_field }}

```
An alternative, and often cleaner, approach is to have multiple 'set' partials and do:

```
{{ my_replicator_field }}
  {{ partial src="sets/{type}" }}
{{ /my_replicator_field }}
```
Then inside your partials directory you could have:

`sets/image_set.html`
`sets/quote_set.html`

and the set partial may look something like:

```
{{# this is image_set.html #}}

{{ assets:image }}
  <img src="{{ glide:id }}" alt="" >
{{ /assets:image }}
```
