---
title: Taxonomies
id: 30955bc8-9c14-4523-858f-01c308b50d63
overview: |
    A "taxonomy" is a system of classifying data around a set of unique characteristics such as category, color, or the way smart scientists have grouped all living creatures into Kingdoms, Species and so on.
---
## Overview {#overview}

---

**Taxonomies have been revamped with the release of Statamic 2.5.**  
These docs assume you're using at least version 2.5.  
If you're upgrading, [check out the differences below](#statamic-2-5).

---

Taxonomies are one of Statamic's 6 core [Content Types][content-types]. As a Content Type it is similar in many ways to Entries and Pages in that each item can have its own fields, unique slugs, and be routed with any supported URL pattern.


## The Taxonomy {#taxonomy}

Each **taxonomy** acts as a container for **terms** in the same way that **collections** contain **entries**.

Each taxonomy has a configuration file located in `site/content/taxonomies`.  
The filename indicates the _handle_. The handle is how you will reference the taxonomy throughout your site.

For example, if you wanted a "Tags" taxonomy, you would create `tags.yaml` file 
(where `tags` would be the handle) which might contain this:

``` .language-yaml
title: Tags
template: tag_index
```

Simple, right?  

Technically, the existence of the file is enough for it to work, but if you're using the Control Panel, you will probably 
want to add at least `title` for it to be displayed correctly.

Any additional fields inside the file will will cascade down to the terms. In the above example, specifying `template`
in the taxonomy is equivalent to you adding that value to each and every one of its terms.


## Taxonomizing Content {#taxonomizing}

Entries should have a field that matches the taxonomy handle *exactly*.

``` .language-yaml
title: My Entry
tags:
  - wonderful
  - fantastic
```

The field should have an array of term _values_. There's no need to create any term files.
In this example, `wonderful` and `fantastic` will now be registered as terms in the `tags` taxonomy.

If using the Control Panel, the fields used for taxonomizing the content will be displayed in the sidebar.
Learn more about [Taxonomies in the Control Panel](#control-panel) below.


## Term Values and Slugs {#values-and-slugs}

A term **value** is how you might identify a term in your content.

A term **slug** is the URL-safe version, and is what Statamic uses internally to track terms.

Take this entry, for example:

``` .language-yaml
tags:
  - Star Wars
```

- Statamic will take the value of `Star Wars`, and work out the _slug_ (which would be `star-wars`).
- If there's already a term with a slug of `star-wars`, that's what it'll use.
- If there's no existing term, it'll create one with a _slug_ of `star-wars` and save the title as the value you entered, which is `Star Wars`.

The titles are saved on a first-come first-serve basis, so you'll want to try to be consistent.

What that means, is if you enter `Star Wars` in one entry, and `star wars` in another, whichever one Statamic happens to encounter first will be used as the title.

To clarify:
`Star wars`, `star wars`, `StAr WaRS`, and `star-wars` are all the same term.

This is the trade-off between the ease of use of quickly entering strings into content versus human error in consistency.

To combat title inconsistencies, you may add a `title` field to a term's [additional data](#additional-data).

## Terms in the Control Panel {#terms-in-the-cp}

When adding terms in the Control Panel:

- Term _slugs_ will _always_ be saved to content.
- Existing terms will be displayed in the field with their title, if there is one.
- If you add a new term, and it doesn't match its slug-to-be, a term file will be created with the title explicitly defined. (See [Additional Term Data](#additional-data) below)


## Templating {#templating}

### Terms {#terms}

Taxonomy term values in your content will be converted to actual Term objects automatically.

If you're [coming from before Statamic 2.5](#statamic-2-5), you may reach for the Relate tag. It's no longer necessary.
However, it will still work just fine, so you don't _need_ to update your templates.

```
{{ tags }}
  {{ title }}, {{ url }}, {{ slug }}, etc
{{ /tags }}
```

The raw array of values can be accessed by appending `_raw`.

```
{{ tags_raw }}
  {{ value }}
{{ /tags_raw }}
```

### Collections {#collections}

Collections may be filtered by taxonomies in several ways.

Specifying the taxonomy and term slugs manually.

```
{{ collection:blog taxonomy:tags="foo|bar" }}
```

Prefixing with a colon will read from a field.

``` .language-yaml
the_tags:
  - foo
  - bar
```

```
{{ collection:blog :taxonomy:tags="the_tags" }}
```

You can specify whether to match *any* or *all* of the provided terms.

```
{{ collection:blog taxonomy:tags:any="foo|bar" }}
{{# Entries with either foo or bar will be matched. #}}

{{ collection:blog taxonomy:tags:all="foo|bar" }}
{{# Entries with both foo and bar will be matched. #}}
```

The default behavior is *any*.

When on a term's URL, simply adding `taxonomy="true"` will intelligently filter by the current term.

```
{{ collection:blog taxonomy="true" }}
{{#
    Assuming you're on a term's URL, eg. /blog/tags/foo
    It'll know to filter by that.
    Equivalent to {{ collection:blog taxonomy:tags="foo" }}
#}}
```


## Additional Term Data {#additional-data}

More data (fields) can be added to a term by creating a yaml file named by the term's [slug](#values-and-slugs).

``` .language-files
site/content/taxonomies/
|-- tags.yaml
`-- tags/
    |-- foo.yaml
    `-- bar.yaml
```

In the YAML file, simply add an array of data, like so:

``` .language-yaml
food: bacon
drink: whisky
```

Whenever referencing the Terms in your templates, now `{{ food }}` and `{{ drink }}` would output `bacon` and `whiskey` respectively.


## Localization {#localization}

### Terms {#localizing-terms}

To localize a term's data, simply add the the translated values nested under the corresponding locale's key within the term's file.

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

The slugs go inside the taxonomy configuration file, nested under their respective locale, using the default slug as the key.

``` .language-yaml
title: Tags
slugs:
  fr:
    red: rouge
  de:
    red: rot
```


## Creating unused terms {#creating-unused-terms}

When iterating over taxonomy terms, only ones that are used within content will be output.

Typically, a term won't exist at all if it hasn't been used within content.

To explicitly create a term without using it in content, you can simply create the file as if you were adding [additional data](#additional-data) mentioned above.

You will still need to add `show="all"` when using the [Taxonomy tags][taxonomy-tag] to output unused terms.


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

To define which taxonomies should be displayed, without any additional configuration, just supply a list of taxonomy handles:

``` .language-yaml
taxonomies:
  - tags
  - categories
```

To customize the fields, add their configuration just like any other fields:

``` .language-yaml
taxonomies:
  tags:
    max_items: 1
  categories: true  # no configuration needed, but still want it shown
```

Note: 

- If you customize one field, you must at least specify true for the others.
- Specifying `type: taxonomy` is not required.


## Statamic 2.5 Changes {#statamic-2-5}

- Terms no longer require their own file. (Unless you want to add [additional data](#additional-data) or need to leverage [unused terms](#unused-terms).)
- Terms no longer need to exist before you add them to content. Just add them.
- Term IDs are no longer used in content. [Terms values](#values-and-slugs) are used.

[content-types]: /content-types
[collection-tag]: /tags/collection
[taxonomy-tag]: /tags/taxonomy