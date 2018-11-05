---
title: Caching
id: 102f8c10-9120-4a6e-a630-97050b494b89
overview: >
  A flat-file CMS wouldn't be worth its salt without a few different caching mechanisms. In this guide we peel back the various layers like an onion and make at least one Shrek joke.
---
## An introduction to flat-file caching {#intro}

In most "traditional" applications (namely those involving a relational database for content and data storage), caching is usually just a performance-enhancing tool. Statamic on the other hand uses a caching layer as a data-source, much in the way a Redis or NoDB application might. We call this layer the _**"Stache"**_.

## The Stache Datastore {#stache}

If Statamic were to search through and read all of your content and settings files every single request, you would probably rage quit the internet or <nobr>(╯°□°）╯︵ ┻━┻)</nobr>, unless your site was very small.

Statamic watches for changes to your content and settings and compiles a number of [ephemeral][ephemeral] data structures that are used to power your website not unlike an API. It is this Stache that allows fast content querying, relationships, routing, search indexing, URL traversing, and really everything else that makes Statamic useful. Think of your content as the permanence layer. As long as it exists, the Stache can always be rebuilt.

### How is it invalidated? {#stache-invalidation}

Since the Stache is temporary and self-replicating, you can delete it to invalidate it or any content inside it anytime if you have need. Statamic will do this automatically when we detect changes to files or explicitly do so from the Control Panel, but since you can customize this behavior, it's good for you to know how to do it yourself.

To clear the Stache (and refresh any content or settings), you can delete `local/storage/framework/cache/stache/` or run `php please clear:stache` via the command line.

### Can I turn it off? {#turn-it-off}

It is _always_ enabled and is powered by magic. You cannot, nor should you want to ever turn it off. If you were Aquaman, would you give away your powers? Didn't think so. Everyone wants to talk to fish.


## System Cache {#system}

The system cache is used by Statamic and addons to store data for defined lengths of time, much like sessions or cookies might do on the browser side. It uses Laravel's [cache][laravel-cache] API. As an example, the Image Transform feature uses this cache to store all the manipulated images and their various sizes.

### How is it invalidated? {#cache-invalidation}

Each item in the system cache invalidates itself over time. Some features, like the Search Index Throttle, can have configured lengths of time, and others pre-set. It's highly unlikely you'll have to deal with this directly unless you're building addons.

If you find the need to invalidate this cache for some reason, you can wipe the contents of the `local/storage/framework/cache` folder or run `php please clear:cache`. It will rebuild itself as needed.

### Can I turn it off?

Nope.

### Blink and Flash {#blink-flash}

Blink and Flash are two types of Request-based caches used by Statamic and addons.

**Blink** stores data for the remainder of the _current request_, allowing other features, tags, or template logic to reuse that data for performance gains. It's similar in concept to SQL caching.

**Flash** sets data for one-time use in the _next request_, like success messages and whatnot.

### How is it invalidated? {#blink-invalidation}

Refresh the page. It's gone. And maybe even back again, who knows? We can't tell from here.

## Template Fragments {#template-fragments}

There are times when you may want to simply cache a _section_ of a template to gain some performance. That's what the [Cache Tag][cache-tag] is for. Wrap your markup in `{{ cache }}` tags and you're off on your way to a snappier, zippier, peppier site.

```
{{ cache for="1 hour" }}
  <!-- SO MUCH STUFF HAPPENING HERE DONKEY!
  ...
  1,000 lines later...
  -->
{{ /cache }}
```

### Can I turn it off? {#frag-off}

If you want to disable all your cache tags, you can set `cache_tags_enabled: false` in `site/settings/caching.yaml`.
This could be useful during development or for debugging.

### How is it invalidated? {#frag-invalidation}

If you specify the cache length, it'll invalidate itself after that length of time.
Leave it off, and it'll remain cached until you manually clear your cache or change the template between the tags.

## Static Caching {#static-page}

Now we get to the **performance** part of the show. There is absolutely nothing faster on the web than static pages (except static pages without javascript and big giant header images, of course). And to that end, Statamic can cache static pages and pass off routing to Apache or Nginx through reverse proxying. It sounds much harder than is.

Certain features, like forms with server-side validation, don't work with static page caching. As long as you understand that, you can leverage this feature for literally the fastest sites possible. Let's take a look!

There are 2 stages of Static caching. Half Measure, and Full Measure.

### Half Measure {#half-measure}

Head to `System » Caching` or `site/settings/caching.yaml` and turn on **Static Caching**. This will still run every request through the full Statamic bootstrapping process but will serve all request data from a cache, speeding up load times often by half or more. This is an easy, one-and-done setting.

```
static_caching_enabled: true
```

### Full Measure {#full-measure}

**Step one:** Head to `System » Caching` or `site/settings/caching.yaml` and turn on **Static Caching** and set the type to **File**. This will start generating full static HTML pages in a `static/` directory in your webroot.

```
static_caching_enabled: true
static_caching_type: file
```

**Step two:** Enable the rewrite rule for whichever server you're running. It's commented out in each of the sample server config files included with Statamic.

Apache:

```
RewriteCond %{DOCUMENT_ROOT}/static/%{REQUEST_URI}_%{QUERY_STRING}\.html -s
RewriteCond %{REQUEST_METHOD} GET
RewriteRule .* static/%{REQUEST_URI}_%{QUERY_STRING}\.html [L,T=text/html]
```

Nginx:

```
location / {
  try_files /static${uri}_${args}.html $uri /index.php?$args;
}
```

IIS:

```
<rule name="Static Caching" stopProcessing="true">
  <match url="^(.*)"  />
  <action type="Rewrite" url="/static/{R:1}_{QUERY_STRING}.html"  />
</rule>            
```

**Step three:** Watch your site fly!

![Very Performance! So Speed!](/assets/img/screenshots/performance.png)

### Query strings {#query-strings}

By default, static caching will _include_ any query strings in the URL. For example, visiting `/about` and `/about?something`
will result in two different pages. This is useful for being able to cache query string dependent pages, like paginated sections. However, it would also cause the cache being broken by someone appending `?whatever` to the URL. 

To disable pagination, set `static_caching_ignore_query_strings: true`. If you're using full measure static caching, you will need to adjust your rewrite rules to ignore the query.

For example:

- Remove `%{QUERY_STRING}` from htaccess if using Apache
- Remove `${args}` from your config if using NGINX
- Remove `{QUERY_STRING}` from web.config if using IIS

### Excluding pages {#excluding-pages}

You may add a list of URLs you wish to exclude from being cached. You may want to exclude pages that need to always be dynamic, such
as forms and listings with `sort="random"`.

``` .language-yaml
static_caching_exclude:
  - /contact
```

Wildcards may be used at the end of URLs.

``` .lang-yaml
static_caching_exclude:
  - /blog/*  # Excludes /blog/post-name, but not /blog  
  - /news*   # Exclude /news, /news/article, and /newspaper
```

**Note:** Query strings will be omitted from exclusion rules automatically, regardless of whether wildcards are used. 
For example, choosing to ignore `/blog` will also ignore `/blog?page=2`, etc.

### Invalidation {#invalidation}

#### Time limit {#invalidation-time}

When using half-measure, you're able to set the number of minutes before the cached pages automatically expire.

``` .language-yaml
static_caching_default_cache_length: 5
```

For full-measure, since the generated html files will be served before PHP ever gets hit, the cache length option is _not_ available.

#### When saving {#invalidation-saving}

When saving content, the corresponding item's URL will be flushed from the static cache automatically.

You may also set specific rules for invalidating _other_ pages when content is saved. For example:

``` .language-yaml
static_caching_invalidation:
  collections:
    blog:
      urls:
        - /blog
        - /blog/category/*
        - /
  taxonomies:
    tags:
      urls:
        - /blog
        - /blog/category/*
        - /
  pages:
    /about:
      urls:
        - *
```

This says:

  - "when an entry in the `blog` collection is saved, we should invalidate the `/blog` page, any pages beginning with `/blog/category/`, and the home page."
  - "when a term in the `tags` taxonomy is saved, we should invalidate those same pages"
  - "when the /about page is saved, invalidate all pages"

Of course, you may add as many collections, taxonomies, and pages as you need.

You may also choose to invalidate the entire static cache by specifying `all`.

``` .lang-yaml
static_caching_invalidation: all
```

#### By force {#invalidation-force}

To clear the static file cache you can run `php please clear:static` (and/or delete the appropriate static file locations).

### File Locations {#file-locations}

When using full measure caching, the static html files are stored in the `static` directory.

You may customize this by changing the `static_caching_file_path` variable.

``` .lang-yaml
static_caching_file_path: static_files
```

You will need to update your appropriate server rewrite rules.

### Localization {#localization}

When using full measure caching combined with localization, you may need to save the html files within each appropriate locale's webroot.

You may do this by placing an array in the [file locations](#file-locations) with the various locales.

``` .lang-yaml
static_caching_file_path:
  en: static
  fr: fr/static
```

## Static Site Generation {#static-generator}

En route from Far Far Away Land. Stay tuned!


[ephemeral]: https://en.wiktionary.org/wiki/ephemeral
[laravel-cache]: http://laravel.com/docs/5.1/cache
[cache-tag]: /tags/cache
