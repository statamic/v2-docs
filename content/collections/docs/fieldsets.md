---
title: Fieldsets
id: 4da38f4b-0154-42ec-873e-35a5bfbafc79
overview: |
  Fieldsets are the tool used by your Control Panel to create publish forms. They establish your content model. Essentially, a schema that defines your fields, data types, and the interface used to manage them.
video: https://youtu.be/EhIZfxYwbRc
---

## What is a fieldset? {#what-is-a-fieldset}

A _fieldset_ is YAML file that defines a list of fields used to create content. They're reusuable, highly configurable, and the cornerstone of a customized and tailor-fit Control Panel.

Your fieldsets are kept in the `site/settings/fieldsets` directory. The main section of a fieldset is the `fields` key which allows you to set and configure any number of fields utilizing any combination of the available [fieldtypes](/fieldtypes).

An example of what a fieldset might look like:

``` .language-yaml
title: Blog Post
fields:
  content:                # The template tag, i.e. {{ content }}
    display: Content      # The CP field label
    type: markdown        # The fieldtype
    instructions: Write!  # Instructional text
    validate: required    # Validation rules
  author:
    display: Author
    type: users
    default: current
    max_items: 1
```

## Using a fieldset {#usage}

To assign a fieldset to a piece of content, add a `fieldset` key to its YAML data.

### Via content {#using-via-content}

You may add it directly to a page, entry, global set, or taxonomy term. This may be useful if you have a need for a single-use fieldset. For example, one page may require a specific fieldset.

### Via container {#using-via-container}

You may also add it to the corresponding content containers (Collection, Taxonomy, Asset Container, etc) which will cascade down to all of its items. For example, adding `fieldset` to a Collection's `folder.yaml` will ensure that all the entries (new and existing) will use that fieldset.

If you've defined a container level fieldset, you may still override it on the content level.

## Configuring a fieldset {#configuration}

### Naming fields {#naming}

You can name your fields any way you choose, but each field name needs to be unique. You cannot use hyphens, but underscores are okay.

### Required field settings {#required-settings}

In the snippet above, the only thing that's really required is the `type` key. Even without that, Statamic will treat it as a [text fieldtype](/fieldtypes/text) by default.

Some fieldtypes will have their own required settings. For example, the [assets fieldtype](/fieldtypes/assets) requires that you specify a container.

### Validation Rules {#validation}

Each field can accept a pipe-delimited list of [Laravel validation rules](https://laravel.com/docs/5.1/validation#available-validation-rules).

For example:

``` .language-yaml
fields:
  something:
    type: text
    validate: required|alpha_dash|between:5,10
```

This would make the field required, ensure only alpha-numeric characters and dashes are used, and that the text is between 5 and 10 characters long.

Note that not _every_ validation rule would be usable in Statamic. For instance, the database rules (exists, unique, etc) would not apply since we do not use a database.

## Sections [Since 2.9.0] {#sections}

You may organize your fields into sections, which will be displayed as tabs on the publish page.

Instead of having a top level `fields` key as explained above, you should have a top level `sections` key, and each one will have its own `fields`. For example:

``` .language-yaml
title: My Fieldset
sections:
  main:
    display: Main
    fields:
      field_1:
        type: text
      etc:
        ...
  another:
    display: Another
    fields:
      field_3:
        ...
```

### Sidebar section

By naming one of your sections `sidebar`, it will be treated as one (when your browser is wide enough).
When your browser is no wide enough to display the sidebar, it will just become another tab. You are free to rename your sidebar section by customizing the `display` key.

``` .language-yaml
sections:
  main:
    fields: ...
  sidebar:
    display: Meta Fields
    fields: ...
```

Without a sidebar, the other sections will become full width. You may prefer it this way!

### Meta Fields

There are some special meta fields that may be automatically be added to your fieldset if you haven't explicitly defined them.

- If you haven't defined a `title` field, one will be added to the top of your first section.
- A `slug` field will be added to the sidebar for almost all publishable content.
- A `date` field will be added to the sidebar if you are editing a date-based collection.
- When editing an entry, [a field for every taxonomy](/taxonomies) will be added to the sidebar.

You are free to add these fields to any section.

``` .language-yaml
sections:
  main:
    fields:
      title:
        display: Post Title
      slug:
        display: Post Slug
      date:
        display: Publish Date
      categories:
        type: taxonomy
        taxonomy: categories
```

## Reusing fields across fieldsets {#partials}

It's fairly common to want to repeat certain fields across multiple fieldsets. In this case, you may use the [Partial fieldtype](/fieldtypes/partial) to include another fieldset. Any of the partial fieldset's fields will be included where you specify.

## Conditional Fields {#conditional-fields}

It's possible to have top-level fields be displayed only under certain conditions. You may specify various rules under either the `show_when` or `hide_when` keys.

> **Important:** Conditionals work on top-level fields only, not meta fields (fields that include other fields) like Replicator, Bard, and Grid.

A simple example might be to show a field when a toggle is set to "on".

``` .language-yaml
title: Blog Post
fields:
  has_author:
    type: toggle
  author:
    type: users
    max_items: 1
    show_when:
      has_author: true
```

If you don't need to toggle it but instead "just show" the author field, you may be interested in the [Revealer fieldtype](/fieldtypes/revealer).

Here's another example using a select field:

``` .language-yaml
title: Blog Post
fields:
  post_type:
    type: select
    options:
      - text
      - image
      - video
  youtube_id:
    type: text
    show_when:
      post_type: video
  image:
    type: assets
    container: main
    max_files: 1
    show_when:
      post_type: image
```

- The `youtube_id` field will only be displayed when the `post_type` field has `video` selected.
- The `image` field will only be displayed when the `post_type` value is `image`.

### Not Nulls {#not-null}

You may specify a value to be "not null" which is handy when you need to say "Show this when another field has any value."

``` .language-yaml
show_when:
  that_field: not null
```

### Multiple fields {#multiple-conditional-fields}

You can combine fields by adding to the `show_when` or `hide_when` array.

The following says: "Show when `this_field` is `bacon` AND `that_field` is `cheeseburger`"

``` .language-yaml
show_when:
  this_field: bacon
  that_field: cheeseburger
```

You can use "OR" rules by prepending fields with `or_`.  
The following says: "Show when `this_field` is `bacon` OR `that_field` is `cheeseburger`"

``` .language-yaml
show_when:
  this_field: bacon
  or_that_field: cheeseburger
```

You may specify multiple values for a single field by using an array for the value.  
The following says: "Show when `this_field` is one of `bacon`, `eggs`, or `hash browns`"

``` .language-yaml
show_when:
  this_field: [bacon, eggs, hash brows]
```

### Custom Logic {#custom-conditional-logic}

If you need something more complex than the YAML syntax provides, you may write your own logic.

In your `site/helpers/cp/scripts.js` file (or within an addon), you should add a function to the `Statamic.conditions` object.

``` .language-javascript
Statamic.conditions.reallyLovesFood = function (fields) {
    return fields['favorite_foods'].length > 10;
};
```

``` .language-yaml
fields:
  favorite_foods:
    type: list
  food_haiku:
    type: textarea
    show_when: reallyLovesFood
```

This will present the user the ability to declare their obvious love for food in the `food_haiku` field if more than 10 favorite foods have been listed.
