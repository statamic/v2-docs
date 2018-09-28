---
title: Performance Optimizing
kb_categories:
  - Tips, Tricks, and How-Tos
id: 302be45f-bd9d-4a26-b836-d0f5c84b4773
---
## Upgrade Statamic

We're continually making updates. We rarely try to make things slower as we go, so make sure to try to keep as
up-to-date as possible.

If you aren't already on at least 2.5, get on it! We made some drastic improvements under the hood.

## Prevent the Stache from always updating

The Stache is our affectionate name for our content caching layer. It's like our "database".

By default, Statamic will look at all your content files for any changes _on every request_. This is what allows you
to edit a markdown file and have it be instantly reflected on your site.

If it doesn't have to do that, you'll save on overhead, especially for larger sites. To disable that, simply add
the following to `site/settings/caching.yaml`:

``` .language-yaml
stache_always_update: false
```

Obviously the drawback to disabling this setting is that making a change won't be reflected automatically. Either clear the
cache or use the CP to trigger updates. Most people might consider leaving it enabled in development and disabled
in production.

## Disable Search Auto-indexing

This setting is actually already disabled by default, although some might find it and think it's an awesome thing so
they enable it!

Just be aware that automatically indexing can eat up some performance whenever it has to do its thing. It may also
take a long time if you have a lot of content.

Turn it off, either make sure the following setting in `site/settings/search.yaml` is `false`, or just not there at all.

``` .language-yaml
auto_index: false
```

## Less assets per folder

Since 2.1, Statamic will lazy load content which means it will only load a particulate bucket of content when it's needed.

Assets is one part of that, and we store your assets in folders.

It's better to have many assets spread across multiple folders instead of all your assets dumped into one folder.

## Smaller templates

A large template (or partial, or layout) can affect performance negatively simply because Statamic will use more memory
to parse a longer string. You can cater to this by splitting your templates up into partials.

Take this example:

```
<html>
  <head>
    ... a ton of meta tags ...
  </head>
  <body>
    <header>
        ... a bunch of navigation ...
    </header>
    <section>
       {{ template_content }}
    </section>
    <footer>
        ... a bunch of links ...
    </footer>
</html>
```

By the time you have all your meta tags, nav tags, footer stuff, and whatever else, your template could easily be
a few hundred lines. Looking at that example, it may already become obvious how you could split this up.

```
<html>
    {{ partial:head }}
    <body>
        {{ partial:navigation }}
        <section>
            {{ template_content }}
        </section>
        {{ partial:footer }}
    </body>  
</html>
```

## Use Redis

[Redis](https://redis.io/) is a NoSQL key/value store, much like our flat file system based on [YAML](/yaml), that is held in memory. This results in very fast I/O response times and lets you scale to much larger and handle heavier traffic. It's a perfect fit for taking Statamic to the next-level.

[Learn how to enable Redis](/knowledge-base/redis-cache)
