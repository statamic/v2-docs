---
title: Partial
overview: Nest other fieldsets inside a fieldset.
options:
  -
    name: fieldset
    type: string
    description: Name of the fieldset to include.
id: 2c61bce9-6671-4d54-bfde-6d02afc8f670
---
## Usage {#usage}

The purpose of this fieldtype is to allow you to create fieldset "partials" that you can include within a larger
fieldset. This is handy when you want to reuse common fields throughout a number of other fieldsets.

A popular example is if you have a `blog_post.yaml`, a `page.yaml`, and you wanted to include some SEO meta fields
in both of those. You could create a `seo.yaml`, and then reference that from within the other fieldsets.

## Example {#example}

The `blog_post.yaml` fieldset, which is what you'll be associating to posts using `fieldset: blog_post`:

``` .language-yaml
fields:
  title:
    type: title
  seo: # this key can be anything as long as it's unique.
    type: partial
    fieldset: seo
  content:
    type: markdown
```

The `seo.yaml` field, which we'll reference as a partial from within `blog_post.yaml` above.

``` .language-yaml
fields:
  meta_description:
    type: text
  meta_keywords:
    type: text
```

When using the `blog_post` fieldset on the publish page, the fields from the partial will be brought inline and 
rendered in the following order, as if they were all part of a single fieldset:

- `title`
- `meta_description`
- `meta_keywords`
- `content`

## Caveats {#caveats}

There are a couple of things worth noting:

- The field name of the partial doesn't matter. It'll be replaced by the fields in the partial.
- If your partial contains the same field names as in another, it will be overridden. It might be a good idea to
  keep them unique.
