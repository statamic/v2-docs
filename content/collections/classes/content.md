---
class: Statamic\Data\Content\Content
title: Content
id: b8e862ac-f057-4a3c-9e8f-f2e13029c789
---
Inheritance: [Data](/addons/api/data)

The `Content` class is an abstract class that encompasses the 4 content types that are meant to be published and/or viewed on the front-end of the website. These are `Page`, `Entry`, `Term`, and `GlobalSet`.

The `Content` class deals with the manipulation of slugs, publish status, URI/URL, templates, and more.

For the purposes of examples, the following code snippets will be performed using a `$page` (`Statamic\Data\Pages\Page`).

## slug

Get or set the slug.

```
$page->slug(); // Returns a string
```
```
$page->slug('my-page'); // Returns null
```

## order

Get or set the order.

```
$page->order(); // Returns string
```
```
$page->order(2); // Returns null
```

## published

Set whether or not the content is published.

```
$page->published(); // Returns a boolean
```
```
$page->published(true); // Returns null
```

## publish

Syntactical sugar for `$page->published(true)`.

```
$page->publish(); // Returns null
```

## unpublish

Syntactical sugar for `$page->published(false)`.

```
$page->unpublish(); // Returns null
```

## uri

Get or set the URI.

This is the "identifying URL" for lack of a better description.  
For instance, where `/fr/blog/my-post` would be a URL, `/blog/my-post` would be the URI.

```
$page->uri(); // Returns a string, eg. "/my-page"
```
```
$page->uri('/my-page'); // Returns null
```

## url

Get the (relative) URL. This will prepend the site root to your URI.

```
$page->url(); // Returns a string, eg. "/my-page"
```

## absoluteUrl

Get the absolute URL. This will prepend your entire site URL to the URI.

```
$page->absoluteUrl(); // Returns a string, eg. "http://yoursite.com/my-page"
```

## template

Get or set the template.

```
$page->template(); // Returns a string
```
```
$page->template('page'); // Returns null
```

## layout

Get or set the layout.

```
$page->layout(); // Returns a string
```
```
$page->layout('mylayout'); // Returns null
```

## folder

Get the folder of the file relative to the content path.

```
$page->folder(); // Returns a string
```

## fieldset

Get or set the fieldset.

```
$page->fieldset(); // Returns a string
```
```
$page->fieldset('page'); // Returns null
```

## contentType

Get the content type.

```
$page->contentType(); // Returns a string, eg. "page" or "entry"
```

## editUrl

Get the URL to the edit in the control panel.

```
$page->editUrl(); // Returns a string
```
