---
title: Collections
overview: >
  Collections are simply a group of Entries, quite similar to Pages in many ways. While each Page
  tends to be unique, Entries all share the same data structure and are "mounted" to a section of the
  site.
id: 9e4bd6e1-0b91-42bc-92df-610ec6dbf3f4
related_reading:
  - eca58f56-9d97-40f8-9df0-b61b92e29722
  - 4da38f4b-0154-42ec-873e-35a5bfbafc79
  - 767fe16a-406f-450b-840b-a7c34ebba465
---
## Overview

Collections can be anything. Blog posts, news articles, knock knock jokes, you name it. Other content management systems might call them "channels", "structures", or "post types".

You can create as many collections as you'd like, each with its own default group of custom fields (called a "Fieldset"). Each Collection can contain any number of Entries with a date or numerical `order_key` and a slug for a filename: `2015-12-24.christmas-eve.md`.

By their very nature, Collections don't determine their own full URLs. To do so would limit their flexibility. Instead, you can "mount" a collection onto any page (or pages) and then write a route to determine their URL structure. This opens many, many, many possibilities. That's three (3) manys, in case you were counting.

Collections are kept in `site/content/collections`, each in their own subfolder, and contain a flat list of Entries in a YAML Front-Loaded format (there's that term again).

## Example

```language-files
collections/
|-- blog/
|   |-- 2015-01-18.my-first-day.md
|   |-- 2015-01-19.paperwork-and-snowshoeing.md
|   |-- 2015-03-08.spring-wonderful-spring.md
|   |-- 2015-05-16.speeder-bikes-and-wookies.md
|-- news/
```

Entries are designed to be viewable each at their own unique URL. There certainly are exceptions to this rule, and no technical limitation preventing you from writing multiple routes and serving the same collections in multiple locations. But be sure to make the right decisions for the quality of your site, because [duplicate content](https://support.google.com/webmasters/answer/66359) is something to avoid when it comes to your search result rankings.

Mounted Collections show up in the Page Tree of the Control Panel as an easy way for content managers to jump directly into publishing, as well as to aid in contextualizing their location within the site.

![Control Panel Collection](/assets/img/screenshots/cp-collection.png)

## Collection-Level Defaults

If you'd like your Entries to inherit some default data, such as a default template, fieldset, or list of taxonomies, you can add that data to the Collections `folder.yaml` file. Each entry can **override** this data if you desire. Anything goes.

``` .language-yaml
# /site/collections/blog/folder.yaml

template: post
show_comments: false
author: jack
```



[nav-tag]: /tags/nav
[taxonomies]: /taxonomies
[glide-tag]: /tags/glide
[taxonomy-fieldtype]: /fieldtypes/taxonomy
[yaml]: /yaml
