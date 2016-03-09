---
title: Taxonomies Are Not Taxidermies
id: 1aea1b4b-cf9b-44a0-92a7-85eba4037283
cover: /assets/img/trail-guides/taxonomies.jpg
description: Classifying and connecting related data goes beyond mere tags with Taxonomies.
overview: |
    A "taxonomy" is a system of classifying data around a set of unique characteristics such as category, color, or the way smart scientists have grouped all living creatures into Kingdoms, Species and so on. On the other hand, "taxidermies" is the rarely used plural of stuffed dead animals.
pre_reqs:
    - The guide on URLs
    - The guide on Collections and Entries or Content Types
state: complete
---
## The Basics

Taxonomies are one of Statamic's 6 core [Content Types][content-types]. As a Content Type it is similar in many ways to Entries and Pages in that each item can have its own fields, unique slugs, and be routed with any supported URL pattern.

Each **taxonomy** acts as a container for **terms** in the same way that **collections** contain **entries**.

Let's take a look at a real world example. For this guide's purposes we'll pretend we're running an apparel company that specializes in shirts, which is a totally unique concept you should probably look into tackling.

Let's assume we have a Shirts collection and we want to be able to filter and sort all of the shirts based on some common characteristics.

```.language-yaml
categories:
  - t-shirt
  - v-neck
  - red
```

## Assigning taxonomies to content

### Using the Control Panel

The recommended way to taxonomize your content is to use the [Taxonomy field](/docs/fieldtypes/taxonomy) in the
Control Panel. This field will save an array of taxonomy term IDs to your content's front matter, like this:

``` .language-yaml
title: "I <3 Whiskey Shirt"
styles:
  - 49083214fsd # the id for the 't-shirt' taxonomy term
  - 8yasdfhiouq # and the 'v-neck' term
```

The Taxonomy field will allow you to choose from existing terms. Of course, this means that the terms should
be created before you try to associate them with your content.

_Note: the ability to add taxonomy terms inline is on our to-do list!_

### Automatic Taxonomy Discovery

If you're coming from v1 you're used to just typing in any old string and having those magically work as taxonomies. You can do this in v2 as well, except that the strings will be replaced with IDs that reference the Term. This lets you add more data to each and every Term if you desire, such as images and descriptions.

To do this you will to map your taxonomy fields to which taxonomies will be held
inside them. You should add something like this to your `site/settings/system.yaml` file:

``` .language-yaml
auto_taxonomy_fields:
  styles: categories
  tags: tags
```

- The `styles` field will hold terms from the `categories` taxonomy
- The `tags` field will hold terms from the `tags` taxonomy

Then simply add the terms to your front-matter. The rest will happen automatically. There are a few different supported formats:

``` .language-yaml
styles:
  - 3e01a0b0      # Existing term IDs
  - t-shirt       # Existing taxonomy term slugs
  - Bacon Merch   # New taxonomy term names
```

- Existing term IDs will be left as-is. They are good to go!
- Existing term slugs are much easier for you to remember than their IDs. They will get converted to IDs.
- Unknown strings will be turned into new taxonomy terms:
  - The string you provide (`Bacon Merch`) will be slugified and used as the slug/filename (`bacon-merch`).
  - The string itself will be used as the `title` field.
  - Of course, the newly created term ID will be replaced.

## Routing Taxonomies

Define the URL format to be used for Taxonomy Terms when editing a Taxonomy. You can use any variable from the Term's data in the route. Most people use `{slug}`, in case you were wondering.

``` .language-yaml
/products/category/{slug}
```

The associated Term URLs will look something like this:

``` .language-output
/products/category/red
/products/category/t-shirt
/products/category/bacon-merch
```

The template used to render `/products/category/{slug}` will come from a cascade:

1. A template explicitly defined in the term's front matter, with `template`.
2. A template explicitly defined in the taxonomy's `folder.yaml`, with `template`.
3. The taxonomy name, eg. `categories.html`.
4. The default taxonomy term template as defined by `default_taxonomy_template` in theming
   settings. By default, this is `taxonomy.html`.
5. The default page template as defined by `default_page_template` in theming settings.  
   By default, this is `default.html`.

## Templating

Iterate over taxonomies in a piece of content using the [Relate tag](/docs/tags/relate).

```
{{ relate:styles }}
    <li>{{ title }}</li>
{{ /relate:styles }}
```

``` .language-output
<li>Red</li>
<li>T-shirt</li>
<li>Bacon Merch</li>
```

[content-types]: /guides/content-types
