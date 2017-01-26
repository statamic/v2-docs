---
title: Taxonomies
id: 30955bc8-9c14-4523-858f-01c308b50d63
overview: |
    A **taxonomy** is a system of classifying data around a set of unique characteristics. Scientists have been using this system for years, grouping all living creatures into Kingdoms, Class, Species and so on. On the web, most often you see Categories and Tags. Think of Taxonomies as the way Statamic _relates content together_ by topic or a shared attribute.
---
## Overview {#overview}

> **Taxonomies have recently been redesigned in Statamic 2.5.**  
[Check out the differences](#statamic-2-5)!

Taxonomies are one of Statamic's 6 core [Content Types][content-types]. They are similar, in many ways, to Entries and Pages. Each Taxonomy can have its own fields, unique slugs, and be routed with supported URL patterns. They differ in that they are not meant to be stand-alone content, but rather describe and group _other_ content.

A prime example of a Taxonomy is "Tags". Entries in your blog are tagged with any and all appropriate terms, each of which exist inside the parent taxonomy "Tags". You then can use those terms to display related content. Want to see all your blog posts about sausage casings? Now you can.


## The Taxonomy {#taxonomy}

Each **taxonomy** acts as a container for **terms** in the same way that **Collections** contain **entries**.

```
Taxonomy -> Term
Collection -> Entry
```

Each taxonomy has a configuration file located in `site/content/taxonomies`.
The filename indicates the _handle_, which is how you access the taxonomy throughout your site and templates.

For example, if you wanted a "Tags" taxonomy, you would create `tags.yaml` file
which might contain this:

``` .language-yaml
title: Tags
template: tag_index

```

The existence of the _file itself_ is enough for Statamic to do its magic, but if you're using the Control Panel, you may want to add a proper `title` for display purposes.

Any additional fields inside this config file will cascade down to the terms themselves as default variables. In the above example, specifying `template`
in the taxonomy is equivalent to setting the template in each and every one of its terms by hand. Except way easier.


## Taxonomizing Content {#taxonomizing}

In order to automatically relate taxonomies and content, your entries need to have a field name that matches the taxonomy handle *exactly*.

``` .language-yaml
title: My Entry
tags:
  - wonderful
  - fantastic
```

The taxonomy field should have an array of term _values_. In this example, `wonderful` and `fantastic` will now be registered as terms in the `tags` taxonomy. That's all there is to it.

In the Control Panel, the taxonomy fields will be displayed in the meta sidebar.
Learn more about [Taxonomies in the Control Panel](#control-panel) below.


## Term Values and Slugs {#values-and-slugs}

A term **value** is how you might identify a term in your content. For example, "Star Wars".

A term **slug** is the URL-safe version, and is what Statamic uses internally to track terms, e.g. `star-wars`. The slug is created automatically based on a few rules. Let's cover them now.

** How we slugify your terms:**

``` .language-yaml
tags:
  - Star Wars
```

- The value `Star Wars` will be converted to lowercase, and all spaces and special characters will be replaced with hyphens: `star-wars`.
- If a term with the slug `star-wars` already exists, the relation is made.
- If no such term yet exists one will be created, and the entered value (`Star Wars`) will become the `title`.

Titles are saved on a first-come, first-serve basis, which means consistency is important. If you enter `Star Wars` in one entry, and `star wars` in another, whichever term Statamic _encounters first_ will be used as the title.

To further clarify,
`Star wars`, `star wars`, `StAr WaRS`, and `star-wars` are all treated as the _same term_. If perfect consistency is important, you can add a `title` field to a term's [additional data](#additional-data).

## Terms in the Control Panel {#terms-in-the-cp}

When adding terms in the Control Panel:

- Term _slugs_ will _always_ be saved to content.
- If a term has a title, it will always be used for display purposes.
- If you add a new term and it doesn't match its slug-to-be, a term file will be created with the title explicitly defined. (See [Additional Term Data](#additional-data) below)


## Templating {#templating}

### Terms {#terms}

Taxonomy term values in your content will be converted to Term objects automatically.

If you're [used to pre v2.5](#statamic-2-5), you may reach for the Relate tag at this point to render your data. Good news! It's no longer necessary. You can simply work with the field name itself. Relate will still work here though, because backwards compatibility is important.

```
{{ tags }}
  {{ title }}, {{ url }}, {{ slug }}, etc
{{ /tags }}
```

If you need to access the un-modified variable data, you can append `_raw` to the variable name.

```
{{ tags_raw }}
  {{ value }}
{{ /tags_raw }}
```

### Collections {#collections}

Collections may be filtered by taxonomies in two primary ways.

1. Specifying the taxonomy and term slugs manually.

```
{{ collection:blog taxonomy:tags="foo|bar" }}
```

2. Specifying a variable name by prefixing with a colon.

``` .language-yaml
the_tags:
  - foo
  - bar
```

```
{{ collection:blog :taxonomy:tags="the_tags" }}
```

You can decide whether to match *any* or *all* of the provided terms with the  `any` and `all` syntaxes. The default behavior is *any*.

```
{{ collection:blog taxonomy:tags:any="foo|bar" }}
{{# Entries with either foo or bar will be matched. #}}

{{ collection:blog taxonomy:tags:all="foo|bar" }}
{{# Entries with both foo and bar will be matched. #}}
```

You may filter by terms in multiple taxonomies by using multiple parameters.

```
{{ collection:blog taxonomy:tags="news" taxonomy:categories="chicken" }}
{{#
    Entries tagged with "news" AND categorized as "chicken" will be matched.
    
    Entries tagged with "news" but not categorized as "chicken" will not be matched.
    The entry must satisfy both parameters.
#}}
```

When on a term's URL (e.g. when matching a Taxonomy route pattern), adding `taxonomy="true"` will automatically filter your collection by the current term.

```
{{ collection:blog taxonomy="true" }}
{{# Equivalent to {{ collection:blog taxonomy:tags="foo" }} #}}
```


## Additional Term Data {#additional-data}

Additional data (custom fields) can be added to a term by creating a yaml file matching the term's [slug](#values-and-slugs).

``` .language-files
site/content/taxonomies/
|-- tags.yaml
`-- tags/
    |-- foo.yaml
    `-- bar.yaml
```

In the YAML file, add data like so:

``` .language-yaml
food: bacon
drink: whisky
```

Whenever referencing the Terms in your templates, now `{{ food }}` and `{{ drink }}` would output `bacon` and `whiskey` respectively.


## Localization {#localization}

### Terms {#localizing-terms}

To localize a term's data you can add the the translated values nested under the corresponding locale key (usually the two character country code), within the term's file.

``` .language-yaml
title: Foo
color: Red
fr:
  title: Le Foo
  color: Rouge
de:
  title: Das Foo
  color: Rot
```

### Slugs {#localizing-slugs}

Term slugs may be localized without the need to create a term file.

The slugs are held inside the taxonomy configuration file, nested under their respective locale, using the default slug as the key.

``` .language-yaml
title: Tags
slugs:
  fr:
    red: rouge
  de:
    red: rot
```


## Creating unused terms {#creating-unused-terms}

When iterating over taxonomy terms, only terms that have been assigned to content/entries will be included. Typically, a term won't exist at all if it hasn't been used within content.

To explicitly create a term without using it in content (to preload options for your writers, for example), you can create the file as if you were adding [additional data](#additional-data) mentioned above.

You will still need to add `show="all"` when using the [Taxonomy tags][taxonomy-tag] to display unused terms on the front-end of your site.


## Control Panel and Fieldtype Usage {#control-panel}

By default, a field for each taxonomy will be displayed in the sidebar of the entry publish page.

You can customize what's displayed by adding a `taxonomies` key to your fieldset. For example:

``` .language-yaml
title: Post
fields: [...]
taxonomies: [...]
```

To show _no_ taxonomy fields, set it to `false`:

``` .language-yaml
taxonomies: false
```

To define which taxonomies should be displayed, without any additional configuration, supply a list of taxonomy handles:

``` .language-yaml
taxonomies:
  - tags
  - categories
```

You can customize them just like any other field:

``` .language-yaml
taxonomies:
  tags:
    max_items: 1
  categories: true  # no configuration needed, but still want it shown
```

Note:

- If you customize one field, you **must** at the very least specify `true` for the others.
- Specifying `type: taxonomy` is not required.


## Statamic 2.5 Changes {#statamic-2-5}

- Terms no longer require their own file unless you want to add [additional data](#additional-data) or need to leverage [unused terms](#unused-terms).
- Terms no longer need to exist before you add them to content. Just add them!
- Term IDs are no longer used in content. [Terms values](#values-and-slugs) are used.

[content-types]: /content-types
[collection-tag]: /tags/collection
[taxonomy-tag]: /tags/taxonomy