---
title: Getting Cozy With Content Types
nav_title: Content Types
cover: /assets/img/trail-guides/content-types.jpg
description: Statamic has six distinct content types. Explore their similarities and their differences.
overview: Content Types are the pillars that hold, contain, and snuggle all of the editable aspects of your site. Each Content Type has its own special attributes that make it unique. In this guide, you get to learn about all six of Statamic's primary Content Types.
id: 1c622c12-93fb-41f5-bd85-239262d1f43f
---
All of your site's content, everything that is managed, edited, uploaded, published and ultimately shown to the world, lives in one of Statamic's six Content Types:

- [Pages](#pages)
- [Collections](#collections)
- [Globals](#globals)
- [Taxonomies](#taxonomies)
- [Assets](#assets)
- [Users](#users)

Each type is a container for any number of items inside that type. For example, you can create any number of Collections, each with its own name, purpose, and data structure, that contains any number of Entries. This pattern is similar in each Content Type.

The individual items in each Content Type are usually represented by content files in a YAML Front-Loaded format. If those words don't mean anything to you - and you're a developer - hang in there and we'll explain everything. If you're a content manager, just ignore 'em! The Control Panel does all this stuff for you.

# Shared Attributes {#shared-attributes}

Content Types are defined by their similarities and their differences. Let's first dive into the well of similarities, drink of its goodness, and be wholly refreshed.

## Customizable Custom Fields {#custom-fields}

The primary attribute that defines a Content Type is the ability to configure any number of **Custom Fields** (called a **fieldset**) to model your content in whatever fashion you desire.

## Universally Unique IDs {#ids}

Universally unique IDs are generated for each item in each type, allowing you to relate items with each other and then retrieve that data later in your template if you so desire. Here are a few examples of how this is useful:

- Associate a User with an Entry creating an author.
- Associate a list of Entries with another Entry to establish Related Posts.
- Include a list of Assets on a page to create a photo gallery complete with pre-established and reusable titles and  captions.
- Add Taxonomy Terms to an Entry to create tags, letting you sort and filter your entries.

# The Differences {#differences}

Now let's explore the differences between Content Types. Each has a distinct purpose and ability that makes them a hero in the world of content management.

## Pages {#pages}

Pages have the unique ability to create URLs based on their folder tree. They are generally used to create the various sections of your site and manage the content of those URLs that are generally static in nature in and of themselves.

Page files are kept in `site/content/pages` and consists of a folder and an `index.md` file.

The order is determined by the `order key` (which consists of the numerical order the page should be sorted in followed by a period) before the rest of the file name. The order key is always stripped out automatically and never shown in the URL. You can set these order keys yourself on the file level, or reorder them via drag and drop in the Control Panel. The choice is yours, but the Control Panel is probably faster and easier.

![Control Panel Page Tree](/assets/img/screenshots/cp-page-tree.png)

Their arrangement can be used by [Nav][nav-tag] and other tags to automatically render your site's navigation markup, if you desire.

Time for a visual.

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

Each page can choose which `template` it should be rendered with. When these URLs are visited Statamic will load the data for the particular page and pass it to the template for rendering. It's nice and logical, unlike YouTube comments.

## Collections {#collections}

Collections are very similar to Pages. Related definitely, twins separated at birth even. But where Pages are the very embodiment of structure -- solidly anchored to their URLs like the ancient redwoods -- Collections are unfettered and float about like colorful kites above the beach waiting to be tied to something. Anything. Multiple things.

Here are a few examples of things that could be their own Collection:

- Products
- Blog Posts
- Portfolio Projects
- News Articles
- Recipes
- Knock-Knock Jokes

You can create as many collections as you'd like, each with its own default fieldset. Each Collection can contain any number of Entries with a date or numerical `order_key` and a slug for a filename: `2015-12-24.christmas-eve.md`.

By their very nature, Collections don't determine their own URLs. To do so would limit their flexibility. Instead, you can `mount` a collection onto any page (or pages) and then write a route to determine their URL structure. This opens many, many, many possibilities. That's three (3) manies, for anyone who didn't count them but simply skimmed the last sentence.

Collections are kept in `site/content/collections`, each in their own subfolder, and contain a flat list of Entries in a YAML Front-Loaded format (there's that term again).

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

## Taxonomies {#taxonomies}

Ah, taxonomies. Simple in practice and yet somewhat difficult to grasp due to their abstract nature. At its basic level, a Taxonomy is a system of classifying data around a set of unique characteristics such as category, color, or subject matter. Twitter and Instagram's #hashtags are a type of Taxonomy, as are traditional tags and categories in a less flexible content management environment.

Any number of Taxonomies can be created, each as its own set of classification, and each containing many Terms. These terms in turn are attached to Entries, giving you the ability to retrieve and filter your content by them. And because Taxonomies are a full-blown content type, each Term can have its own custom fields, letting you store information and assets such as descriptions and photos to be displayed later in your templates.

Taxonomies are routed in a fashion similar to Entries, except they aren't mounted to a page. Instead, they live in their own section of the Control Panel and can be created on the fly with the [Taxonomy Fieldtype][taxonomy-fieldtype] or in your YAML when working with Entries.

[Learn more about Taxonomies][taxonomies]

## Globals {#globals}

Globals are significantly flatter than the preceding Content Types. Instead of being a container for groups of items, each Global Scope is a single set of fields. The difference being that all of the data across all of the Global Scopes is available everywhere in your templates and never tied to an entry or URL in any way.

Globals are perfect for reusable content like:

- Company info
- Footer and sidebar content
- Site settings (think features connected to on/off switches)
- Customer quotes or testimonials
- 404 and form error messaging content

You can create multiple Globals sets in `site/content/globals`, or `Configure » Content » Globals` in the control panel.

Each set/file becomes it's own scope, allowing you to group your Globals logically. All of the variables inside a `footer.yaml` file would be accessed like `{{ footer:var_name }}`. The only Globals without a scope are those found in the default `global.yaml` file.

## Assets {#assets}

Assets are files, whether images, videos, documents, zip files, or anything else within reason you want to upload, manage, and display on your site. Assets are grouped into Containers, allowing you to keep your files organized.

Containers can live in a number of locations and mounted onto Statamic:

- Local filesystem
- Amazon S3 bucket
- Dropbox
- S/FTP server

![Assets Container](/assets/img/screenshots/cp-assets.png)

Asset Containers can use fieldsets to store additional data along with each asset that will be available anywhere your asset is being used, in addition to file-driven metadata. Here are some ideas on how you can use this to your advantage:

- Images can have their own alt tags, captions, and display controls.
- Videos can have transcripts, descriptions, and credits.
- Zip files can have descriptions and MD5sums.

On top of all that, image assets can be manipulated and resized on the fly with our [Image Manipulation API][image-api].

## Users {#users}

Lastly we come to Users. A non-traditional Content Type to be sure, but one nevertheless. Users, while certainly capable of storing authentication information used to access the  Control Panel, they can also be so much more (and less).

User data can be routed and displayed on the site much like Entires, allowing you to use them as a single location for all data pertaining to a specific person.

You can create listings and single view URLs, like with Entries, and group them into User Groups which have Roles you can assign Permissions to. There are so many things you can accomplish with Users that we simply must recommend you read the entire [Guide on Users][guide-users].

[nav-tag]: /reference/tags/nav
[taxonomies]: /guides/taxonomies
[image-api]: #
[taxonomy-fieldtype]: #
[guide-users]: #
