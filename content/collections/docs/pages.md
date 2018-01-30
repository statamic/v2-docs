---
title: Pages
overview: >
  Pages have the ability to create URLs based on how they are arranged in your folder tree. They are generally used to create the main sections and stand-alone pages in your site.
related_reading:
  - 9e4bd6e1-0b91-42bc-92df-610ec6dbf3f4
  - 4da38f4b-0154-42ec-873e-35a5bfbafc79
  - 767fe16a-406f-450b-840b-a7c34ebba465

id: eca58f56-9d97-40f8-9df0-b61b92e29722
---
## Overview

In the Control Panel, pages are represented by a navigation tree. They can be dragged around and reordered, which will be reflected in the URL structures of your site as well as your nav itself (if you're using the [Nav Tag][nav-tag]).

![Control Panel Page Tree](/assets/img/screenshots/cp-page-tree.png)

Page files themselves are kept in `site/content/pages` and each consists of a folder and an `index.md` file.

Their **order** is determined by the `order key` (a number followed by a period) before the rest of the filename. The order key is stripped out of the URL automatically. You can set these order keys yourself on the file level, or order them via drag-and-drop in the Control Panel. The choice is yours, but the Control Panel is probably faster and simpler.

Let's look at some examples.

## Example

```language-files
pages/
|-- index.md
|-- 1.company/
|   |-- index.md
|   |-- 1.team/
|   |-- 2.pets/
|-- 2.services/
|   |-- index.md
|-- 3.about/
|   |-- index.md
|   |-- 1.contact/
```

As you can see, there are a few folders and subfolders, each with their respective `index.md` file. Based on this structure, the following URLs would exist:

- /
- /company
- /company/team
- /company/pets
- /services
- /about
- /about/contact

Each page can choose which template it should be rendered with. When a Page URLs are visited, Statamic will load the page's data and pass it to the template for rendering. It's nice and logical, unlike YouTube comments.

``` .language-yaml
title: About Us
template: about
```

[nav-tag]: /tags/nav
[taxonomies]: /taxonomies
[glide-tag]: /tags/glide
[taxonomy-fieldtype]: /fieldtypes/taxonomy
[yaml]: /yaml
