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

It's also possible to use terms in your content without "taxonomizing" it. [More details below](#without-taxonomizing)


## Term Values and Slugs {#values-and-slugs}

A term **value** is how you might identify a term in your content. For example, "Star Wars".

A term **slug** is the URL-safe version, and is what Statamic uses internally to track terms, e.g. `star-wars`. The slug is created automatically based on a few rules. Let's cover them now.

**How we slugify your terms:**

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

## Term IDs {#ids}

A term ID is simply the taxonomy handle and the slug joined by a slash.  
For example, the `Star Wars` term's ID would be `tags/star-wars`.

Most of the time, you don't need to worry or care about IDs. The only time you might is if you're writing an addon, or referencing terms [without taxonomizing](#without-taxonomizing).


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

Collections may be filtered by taxonomies in a number of ways.

#### Specifying the taxonomy and term slugs manually {#filter-manually}

```
{{ collection:blog taxonomy:tags="foo|bar" }}
```

This will get all blog entries where the tags contain either foo or bar.

#### Specifying a variable name by prefixing with a colon {#filter-from-variable}

The following works just like the previous example except instead of hardcoding the terms into the parameter,
you reference a variable.

``` .language-yaml
the_tags:
  - foo
  - bar
```

```
{{ collection:blog :taxonomy:tags="the_tags" }}
```

#### Specifying the match type {#filter-match-type}

You can decide whether to match *any* or *all* of the provided terms with the  `any` and `all` syntaxes. The default behavior is *any*.

The following will output entries tagged with _either_ foo or bar.

```
{{ collection:blog taxonomy:tags:any="foo|bar" }}
```

The following will output entries tagged with _both_ foo and bar.

```
{{ collection:blog taxonomy:tags:all="foo|bar" }}
```

#### Filter by terms in multiple taxonomies by using multiple parameters {#filter-multiple-parameters}

```
{{ collection:blog taxonomy:tags="news" taxonomy:categories="chicken" }}
```
Entries tagged with "news" AND categorized as "chicken" will be matched.
    
Entries tagged with "news" but not categorized as "chicken" will not be matched.
The entry must satisfy both parameters.

#### Filter by term IDs {#filter-term-ids}

If you have a list of taxonomy term IDs you wish to filter a collection by, you can omit the second part of the parameter.
Your data may be formatted this way when using terms [without taxonomizing](#without-taxonomizing).

``` .language-yaml
things:
  - tags/foo
  - tags/bar
  - categories/news
```

```
{{ collection:blog :taxonomy="things" }}
```

This will get entries in the blog collection where tags contains either foo or bar, or where categories contains news.

You may also hardcode a pipe delimited list of term IDs instead of referencing a variable. Just omit the colon prefix.

```
{{ collection:blog taxonomy="tags/foo|tags/bar|categories/news" }}
```

#### Filter by current URL {#filter-url}

When on a term's URL (e.g. when matching a Taxonomy route pattern), adding `taxonomy="true"` will automatically filter your collection by the current term.

```
{{ collection:blog taxonomy="true" }}
```

For example, if your taxonomy route is defined as `/tags/{slug}` and you are on `/tags/foo`, this will output entries that are tagged with `foo`.

This is equivalent to `{{ collection:blog taxonomy:tags="foo" }}`

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


## Using terms without taxonomizing {#without-taxonomizing}

You're free to reference terms in your content without necessarily taxonomizing (or "tagging") it.

For instance, you may want to have a field in your entry called `similar_tags` which holds a list of
terms that you want to show in a template, but you don't want _this_ entry to be tagged as one of them.

When using this technique, Statamic will _not_ automatically convert your field values to term objects since the
field name you choose will be completely arbitrary.

For example:

``` .language-yaml
---
title: My Entry
tags: 
  - coffee
  - espresso
similar_tags: 
  - tea
---
```

When doing this, you _will_ need to use the [Relate Tag][relate-tag].  
However, in the example above, there's nothing informing Statamic which taxonomy `tea` is a part of. Simply adding a `taxonomy="tags"` parameter
to the tag is enough to push it in the right direction.

```
{{ relate:similar_tags taxonomy="tags" }} ... {{ /relate:similar_tags }}
```

Alternatively, instead of writing term values (or slugs), you can reference the [term IDs](#ids).  
For example, instead of `tea`, you could write `tags/tea`. In this case, the Relate tag will be able to automatically determine the taxonomy. No need to add the parameter.

When using the [taxonomy fieldtype](/fieldtypes/taxonomy) for this, the Control Panel will save term IDs for you.


## Statamic 2.5 Changes {#statamic-2-5}

- Terms no longer require their own file unless you want to add [additional data](#additional-data) or need to leverage [unused terms](#creating-unused-terms).
- Terms no longer need to exist before you add them to content. Just add them!
- Term IDs (at least, those crazy UUIDs) are no longer used in content. [Terms values](#values-and-slugs) are used.

[content-types]: /content-types
[collection-tag]: /tags/collection
[taxonomy-tag]: /tags/taxonomy
[relate-tag]: /tags/relate
