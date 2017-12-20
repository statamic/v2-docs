---
title: Content Types
id: 4076fea7-63c9-4b1c-b16d-7eb565040d81
overview: >
  All content is stored in one of six content types. They all share a common data format (YAML/Markdown files), but each has <span class="highlight">unique characteristics</span> to optimize and simplify the way you manage the content. For example, Pages automatically generate Navigation and Globals are made available in all of your templates.
content_types:
  - db0ae4e3-4f10-4802-bc40-0b880cbf02c7
  - 9e4bd6e1-0b91-42bc-92df-610ec6dbf3f4
  - e8092f11-c3b4-477a-a3a4-9c5e65f45cc4
  - 30955bc8-9c14-4523-858f-01c308b50d63
  - 28bf911b-8f47-489b-8040-37607733b270
  - d744ff59-3507-419d-9808-84079d8111a4
parse_content: true
---

## Overview {#overview}

All of your site's content -- everything created, edited, uploaded, published, and available to display somewhere on your site, lives in one of Statamic's six Content Types:

1. [Pages](/pages)
1. [Collections](/collections)
1. [Globals](/globals)
1. [Taxonomies](/taxonomies)
1. [Assets](/assets)
1. [Users](/users)

## The Lingo (container and child names) {#lingo}

Each content type is a **container** for any number of items. For instance, you can create any number of **Collections** -- each with its own name, purpose, and data structure -- that contain an unlimited number of **Entries**. This "belongs to" pattern exists in each Content Type, not unlike SAT Analogy Questions. Do you remember those? They were super annoying.

The individual items in each Content Type are (in most cases) represented by content files in a [YAML Front-Loaded][yaml] format. Each container and corresponding child type has their own pair of labels.

| Content Type        | Child Type      |
|---------------------|-----------------|
| **Pages**           | Page            |
| **Collection**      | Entry           |
| **Global**          | Set             |
| **Taxonomy**        | Term            |
| **Asset Container** | Asset           |
| **Users**           | User            |

## Each Content Type has this stuff in common {#shared-attributes}

Content Types are defined by their differences, but they share a whole lot of features and attributes. Let's cover these first because they're pretty important -- and useful.

### Custom Fields

The primary attribute that defines a Content Type is the ability to configure any number of **Custom Fields** (in a group called a [**fieldset**](/fieldsets)) to model and structure your content however you see fit. Whether you want one giant text field or a hundred little granular fields and options, you're doing it right.

### Unique IDs {#ids}

Each content type has it's own method of ensure "uniqueness", which lets you create relationships or simply fetch content by ID. Here are a couple of examples of how this is useful:

- Relate a User with an Entry, thereby creating an **author**.
- Relate a list of Entries with another Entry establishing **related articles**.
- Relate a group of Assets with a Entry, creating a **gallery**.

## The Types {#list}

Now let's explore the differences between Content Types. Each has a distinct purpose and ability that makes them a hero in the world of content management.

<div class="flex flex-wrap -mx-1 mb-3">
  {{ relate:content_types }}
    <div class="p-1 w-1/2 lg:w-1/3">
        <a href="{{ url }}" class="bg-grey-lightest hover-lift block p-2 rounded h-full">
            <span class="text-lg font-bold text-pink">{{ title }}</span>
        </a>
    </div>
  {{ /relate:content_types }}
</div>


[nav-tag]: /tags/nav
[taxonomies]: /taxonomies
[glide-tag]: /tags/glide
[taxonomy-fieldtype]: /fieldtypes/taxonomy
[yaml]: /yaml
