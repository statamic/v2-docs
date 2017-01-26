---
title: Taxonomy
extends: 9dd58c40-6e33-49c8-83fa-61a69f6371be
description: Attach Taxonomy Terms to your content.
overview: >
  Allows you attach Taxonomy Terms to your content. Tagging, Categories, Colors, Flavors, you name it. Taxonomy all the things!
image: 0c490534-470b-4255-a19a-29739fef10cf
options:
  -
    name: taxonomy
    type: string
    description: >
      The handle of the Taxonomy from which to fetch Terms. Not needed when placed in the fieldset's `taxonomies` array. 
      In that case, it'll get the taxonomy from the field name.
  -
    name: create
    type: boolean *true*
    description: >
      By default you may create new terms. Set to `false` to only allow selecting from existing terms.
id: 31adcc00-4fbb-4fe9-9b48-401061273096
---

## This is a special field! {#special}

If taxonomizing content (this is what most people use this field for), you should place this field within the `taxonomies` key in your fieldset. [More details](/taxonomies#control-panel)

If not being used for taxonomizing ([more details](#without-taxonomizing)), put it in your `fields` array as usual.

## Data Structure {#data-structure}

If the field is an actual taxonomy and used for taxonomizing (ie. the field name matches the taxonomy handle) then
the term's slugs will be saved.

``` .language-yaml
wildlife:
  - kangaroo
  - three-toed-sloth
```

Otherwise, the term's IDs will be saved. See [below](#without-taxonomizing) for more detail.

A term ID is the taxonomy handle combined with the slug.
This way, you may reference terms from multiple taxonomies.

``` .language-yaml
things_you_may_find_adorable:
  - wildlife/panda
  - people/the-elderly
```

## Templating {#templating}

As outlined in the [Taxonomies Guide](/taxonomies#templating), term slugs will automatically be converted to Term objects which means
you will have all of the term's data available as variables.

```
<ul>
  {{ wildlife }}
    <li><a href="{{ url }}">{{ title }}</a></li>
  {{ /wildlife }}
</ul>
```

``` .language-output
<ul>
  <li><a href="/wildlife/kangaroo">Kangaroo</a></li>
  <li><a href="/wildlife/three-toed-sloth">Three Toed Sloth</a></li>
</ul>
```

## Using terms without taxonomizing {#without-taxonomizing}

The usual usage for taxonomies is to "taxonomize" some content. Or, "to tag content".

However, sometimes you just want to pick some additional terms. For instance you might have a "similar tags" field. 
Or perhaps on a Page you'd like to select which terms you want to be displayed somewhere.

In this case, you aren't really "taxonomizing" (or "tagging") the content.

When using the taxonomy field in this way, terms will get saved using IDs instead of slugs.

```
similar_things:
  - categories/hats
  - tags/delightful
```

These will _not_ be automatically converted to Terms, since the field name does not match any taxonomy handle.
You will need to use the [Relate Tag](/tags/relate) in your template to have these converted to Terms.

```
<ul>
  {{ relate:similar_things }}
    <li><a href="{{ url }}">{{ title }}</a></li>
  {{ /relate:similar_things }}
</ul>
```

``` .language-output
<ul>
  <li><a href="/categories/hats">Hats</a></li>
  <li><a href="/tags/delightful">Delightful</a></li>
</ul>
```