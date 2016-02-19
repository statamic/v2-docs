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
## Usage

The purpose of this fieldtype is to allow you to create fieldset "partials" that you can include within a larger
fieldset. This is handy when you want to reuse common fields throughout a number of other fieldsets.

A popular example is if you have a `blog_post.yaml`, a `page.yaml`, and you wanted to include some SEO meta fields
in both of those. You could create a `seo.yaml`, and then reference that from within the other fieldsets.
