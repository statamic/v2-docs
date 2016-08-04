---
title: Data Retrieval and Manipulation
nav_title: Managing Data
id: bbe3864c-ad6d-4922-84cf-b48586f12bf8
overview: >
  One of the most crucial aspects of extending a Content Management System is being able to retrieve the data and manipulate it. The Statamic API and it's related classed provides you with ways to handle these sort of situations.
---

Consider the various aspects of Statamic: Pages, Entries, Terms, Globals, and Assets. They are all `Data`. Data can have variables/fields that you can `get`, `set`, etc.

Some of these are also `Content`. Content can have URLs, slugs, publish statuses, etc.

Here's the inheritance structure:

``` .language-files
Data
|-- Content
|   |-- Page
|   |-- Entry
|   |-- Term
|   `-- GlobalContent
|-- Asset
`-- User
```

## Retrieving Data

You should retrieve data using our API methods. If you've used Laravel, it should feel similar to Eloquent. If it helps, try thinking of each Data type mentioned above as a Model. We have an API class for each of those.

For example, this will find a page with an ID of `1`.

```
Statamic\API\Page::find(1);
// Returns an instance of \Statamic\Contracts\Data\Pages\Page
```

Like Laravel, if you're expecting a collection of models, you will receive a collection. However, Statamic will give you a subclass like `EntryCollection` which will do everything `Illuminate\Support\Collection` does [(docs)](https://laravel.com/docs/5.1/collections), with a few more contextual methods at your disposal should you need them.

If you're expecting a single model (like `Page::find(1)` above), you'll get the corresponding class.

## Manipulating Data

Once you have a data instance, you can go to town on it.

```
$page->set('foo', 'bar');
```

This is like adding `foo: bar` to the front-matter of the page file.

Once you're done, go ahead and save it.

```
$page->save();
```

Now it'll be written to file. Nice.

## Creating Data

Of course, the data had to get there somehow. You can also create data using the corresponding API classes.

Each of them has a `create()` method that will give you a [factory class][factories]. You can use that factory to build up your object.

```
use Statamic\API\Page;

$page = Page::create('/about') // The pages factory accepts a URI.
          ->order(2)
          ->published(true)
          ->with(['title' => 'About us', 'subtitle' => 'We are awesome'])
          ->get();
```

That `get()` at the end is how you get your newly built object  from the factory. Now you have a shiny new `Page`. Keep in mind that it doesn't exist anywhere until you `save()` it.

**Note:** Never manually do `new Page`. Use the factories.

[Read more about Data Factories][factories]

## Localizing Data

To add localized data to an object, first target it "`in`" the locale.

```
$page->in('fr'); // returns a LocalizedData class
```

A `LocalizedData` class simply holds a locale and the object, and will pass any actions onto the object while targeting that locale.

```
$page->in('fr')->set('foo', 'le bar');
```

All the different locales are stored on the object itself. So when it comes time to `save()` it, Statamic will generate the appropriate files for you.

For example, `$page->save()` would write both `index.md` and `fr.index.md` for you.

**Note:** It's worth noting that even though the `Data` class can hold data in multiple locales, some data types are not (yet) capable of being localized.


# Content

Content is an extension of Data. Content is generally data that will be accessible on the front end. Pages, Entries, Taxonomy Terms, and Globals are all Content.

## URI

When we talk about URIs in Statamic, we are referring to the identifying part of the URL that comes _after_ the root.

For example, a page that exists at `/carrot` has a URI of `/carrot`. Simple.

However you may have that page localized in Spanish as `/es/zanahoria`. The URI is `/zanahoria`. Notice that the locale prefix is omitted. This is because the root is `yoursite.com/es`.

**When retrieving data in Statamic using URIs, you should always use the default locale.**

```
$uri = \Statamic\API\URL::getDefaultUri('es', '/zanahoria'); // returns /carrot
```

## URL

Similar to a URI, a URL is the location from the domain root.

Using the example above, the URLs would be `/carrot` and `/fr/zanahoria`, for English and Spanish, respectively.

Absolute URLs (or permalinks) include the domain. They would be `http://yoursite.com/carrot` and `http://yoursite.com/fr/zanahoria`.

[factories]: /addons/data-factories
