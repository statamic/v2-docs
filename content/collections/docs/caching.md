---
title: Caching
id: 102f8c10-9120-4a6e-a630-97050b494b89
overview: >
  A flat-file CMS wouldn't be worth its salt without a few different caching mechanisms. In this guide we peel back the various layers like an onion and make at least one Shrek joke.
---
## An introduction to flat-file caching

In most "traditional" applications (namely those involving a relational database for content and data storage), caching is usually just a performance-enhancing tool. Statamic on the other hand uses a caching layer as a data-source, much in the way a Redis or NoDB application might. We call this layer the _**"Stache"**_.

## The Stache Datastore {#stache}

If Statamic were to search through and read all of your content and settings files every single request, you would probably rage quit the internet or (╯°□°）╯︵ ┻━┻), unless your site was very small.

Statamic watches for changes to your content and settings and compiles a number of [ephemeral][ephemeral] data structures that are used to power your website not unlike an API. It is this Stache that allows fast content querying, relationships, routing, search indexing, URL traversing, and really everything else that makes Statamic useful. Think of your content as the permanence layer. As long as it exists, the Stache can always be rebuilt.

### How is it invalidated?

Since the Stache is temporary and self-replicating, you can delete it to invalidate it or any content inside it anytime if you have need. Statamic will do this automatically when we detect changes to files or explicitly do so from the Control Panel, but since you can customize this behavior, it's good for you to know how to do it yourself.

To clear the Stache (and refresh any content or settings), you can delete `local/storage/framework/cache/stache` or run `php please clear:stache` via the command line. (Alternatively, you could also delete `local/storage/framework/cache/` or run `php please clear:cache` via the command line. However, *that* would clear not just the stache, but also the larger cache, of which the stache is just one part.)

### Can I turn it off?

It is _always_ enabled and is powered by magic. You cannot, nor should you want to ever turn it off. If you were Aquaman, would you give away your powers? Didn't think so. Everyone wants to talk to fish.


## System Cache {#system}

The system cache is used by Statamic and addons to store data for defined lengths of time, much like sessions or cookies might do on the browser side. It uses Laravel's [cache][laravel-cache] API. As an example, the Image Transform feature uses this cache to store all the manipulated images and their various sizes.

### How is it invalidated?

Each item in the system cache invalidates itself over time. Some features, like the Search Index Throttle, can have configured lengths of time, and others pre-set. It's highly unlikely you'll have to deal with this directly unless you're building addons.

If you find the need to invalidate this cache for some reason, you can wipe the contents of the `local/storage/framework/cache` folder. It will rebuild itself as needed.

### Can I turn it off?

Nope.

### Blink and Flash {#blink-flash}

Blink and Flash are two types of Request-based caches used by Statamic and addons.

**Blink** stores data for the remainder of the _current request_, allowing other features, tags, or template logic to reuse that data for performance gains. It's similar in concept to SQL caching.

**Flash** sets data for one-time use in the _next request_, like success messages and whatnot.

### How is it invalidated?

Refresh the page. It's gone. And maybe even back again, who knows? We can't tell from here.

## Template Fragments {#template-fragments}

There are times when you may want to simply cache a _section_ of a template to gain some performance. That's what the [Cache Tag][cache-tag] is for. Wrap your markup in `{{ cache }}` tags and you're off on your way to a snappier, zippier, peppier site.

```
{{ cache }}
  <!-- SO MUCH STUFF HAPPENING HERE DONKEY!
  ...
  1,000 lines later...
  -->
{{ /cache }}
```

### Can I turn it off?

Nope, just don't use it if you don't want it.

### How is it invalidated?

Aw man, we were hoping you wouldn't ask that. We'll have a smart or automatic method soon, but for now you could change the markup a little bit (which creates a new cache hash) or manually wipe the System cache.

## Static Caching {#static-page}

Now we get to the **performance** part of the show. There is absolutely nothing faster on the web than static pages (except static pages without javascript and big giant header images, of course). And to that end, Statamic can cache static pages and pass off routing to Apache or Nginx through reverse proxying. It sounds much harder than is.

Certain features, like forms with server-side validation, don't work with static page caching. As long as you understand that, you can leverage this feature for literally the fastest sites possible. Let's take a look!

There are 2 stages of Static caching. Half Measure, and Full Measure.

### Half Measure

Head to `System » Caching` or `site/settings/caching.yaml` and turn on **Static Caching**. This will still run every request through the full Statamic bootstrapping process but will serve all request data from a cache, speeding up load times often by half or more. This is an easy, one-and-done setting.

```
static_caching_enabled: true
```

### Full Measure

**Step one:** Head to `System » Caching` or `site/settings/caching.yaml` and turn on **Static Caching** and set the type to **File**. This will start generating full static HTML pages in a `static/` directory in your webroot.

```
static_caching_enabled: true
static_caching_type: file
```

**Step two:** Enable the reverse proxy setting for whichever server you're running. It's commented out in each of the sample server config files (nginx.conf, .htaccess, and web.config) included with Statamic.

**Step three:** Watch your site fly!

![Very Performance! So Speed!](/assets/img/screenshots/performance.png)

### Query strings

By default, static caching will _ignore_ any query strings in the URL. For example, visiting `/about` and `/about?something`
will result in the same page being shown. This is useful for preventing the cache being broken by someone appending
`?whatever` to the URL. However, it will break any functionality that relies on query strings, like pagination.

_Note that when using `static_caching_type: file`, query strings will **always** be ignored._

### Excluding pages

You may add a list of URLs you wish to exclude from being cached. You may want to exclude pages that need to always be dynamic, such
as forms and listings with `sort="random"`.

``` .language-yaml
static_caching_exclude:
  - /contact
```

### Invalidation

When using half-measure, you're able to set the number of minutes before the cached pages automatically expire.

``` .language-yaml
static_caching_default_cache_length: 5
```

For full-measure, since the generated html files will be served before PHP ever gets hit, the cache length option is _not_ available.

You may also set specific rules for invalidating pages when entries in collections have been saved. For example:

``` .language-yaml
static_caching_invalidation:
  collections:
    blog:
      urls:
        - /blog
        - /blog/category/*
        - /
```

This says "when an entry in the `blog` collection is saved, we should invalidate the `/blog` page, any pages beginning with `/blog/category/`, and the home page."
Of course, you may add as many collections as you need.

To clear the static file cache you can run `php please clear:static` or delete the `static/` folder in the web root.

## Static Site Generation {#static-generator}

En route from Far Far Away Land. Stay tuned!


[ephemeral]: https://en.wiktionary.org/wiki/ephemeral
[laravel-cache]: http://laravel.com/docs/5.1/cache
[cache-tag]: /tags/cache
