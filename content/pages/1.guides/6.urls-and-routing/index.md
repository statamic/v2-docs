---
title: URLs and Routing and You Can Too!
cover: /assets/img/trail-guides/urls.jpg
state: complete
description: >
  URLs and pages can be created in a
  number of flexible ways. Learn them all!
id: 44aa1004-e06c-40ec-be2d-0b9e2fa980dc
---
A website is nothing if not a collection of URLs that each display some sort of content. Luckily, Statamic is nothing if not a tool to create and manage URLs. With that small victory established, there are two primary methods of creating and managing your site's URLs.

# Pages {#pages}

Pages are a wonderfully literal representation of your site's URL structures. Each Page lives in hierarchy among other Pages. Together they form a logical tree structure made of parents, children, and siblings. If you wanted to be semantic and overly detailed with the analogy you would be forced to acknowledge the grandparents, cousins, aunts, uncles, nephews, and probably that weird next-door neighbor who invites himself to all of your parties and calls himself **<FEED.XML>**. Really? All caps?

![Hark! Pages!](/assets/img/screenshots/cp-page-tree.png)

Each page has its own `slug`, and those slugs team up based on hierarchy to create URLs. Let's walk through an example and then we'll have a visual. Let's assume we have a restaurant that isn't failing, doesn't need Gordon Ramsey or Robert Irvine to save it, and is booked solid 7 nights a week with people desperate to eat your delicious yum yums.

Naturally, we decide to create a **Menu** page on the website and give it the slug: `menu`. Next we create two new sub-pages called **Specials** with the slug `specials` and **Wine List** with the slug `wine-list`. Mission accomplished.

Because of that parent/child relationship, the following URLs are now available on the site, each with its own unique content:

- `http://example.com/menu`
- `http://example.com/menu/specials`
- `http://example.com/menu/wine-list`

## Slugs {#slugs}

As you may have guessed from context, a slug is the bit of the URL unique to a particular piece of content. You've just seen how the `specials` and `wine-list` slugs combined with their parent page's slug, `menu`, to create full and unique URLs.

> Slugs are primarily used to make URLs more user-friendly and help your users know what to expect before clicking a link. Search engines rank pages higher if the search word is in the URL. It's a scientific fact.

Slugs should always be lowercase, never contain spaces, and almost always use hyphens to separate words. Follow those rules blindly and you'll be just fine.

Pages, Entries, and Taxonomy Terms all have slugs. That sentence sounds gross out of context. Feel free to learn more about all of [Statamic's Content Types](/guides/content-types) - including those with and those without slugs.

# Routes {#routes}

Routes are a common pattern in the world of web applications, but not one found often in content management. Traditionally a route is nothing more than a URL pattern mapped to a handler - something built to tell the application what to show or render, like a controller, static page, or something of that nature. Naturally, we decided to take it a step further. Let's get routing.

First of all, routes are managed in `site/settings/routes.yaml` or in the Control Panel settings area. Inside you'll find the ability to create three different types of routes.

## Template Routes {#template-routes}

Template routes allow you to create individual URLs or patterns that map to specific templates. This is very different than using a Page to create a URL because there is no inherent content manageable in the Control Panel or content files. Instead, you can code anything you need to into the template itself, use [Tags](/docs/tags) and [Globals](/guides/content-types/#globals) to display content, or pass data in through the route itself.

This is a useful technique for creating pages that aren't editable in the Control Panel, or where an empty page might be more confusing than helpful. RSS feeds, site maps, search results, and other utility pages are a few such examples.

To create a template route set the desired URL or pattern as a key under `routes`, and then the template and any additional variables or settings you wish to pass through in a key-value array.

``` .language-yaml
/search:
  template: search-results
  title: Look What I Found!
  show_sidebar: false
```

When visiting [example.com/search](), the `search-results.html` template will be rendered and  `{{ title }}` will display **Look What I Found!**, and whatever you decided `{{ if show_sidebar }}` should do, won't be happening. You just shut that party down.

If all you need to set is a template, you can simplify the syntax and just pass the template string instead of the array.

``` .language-yaml
/search: search-results
```

### Wildcard Segments {#wildcard-segments}

Wildcards let you capture multiple routes at once. It's like killing infinity birds with one stone. Wildcards are available anywhere in your route pattern and there aren't any limits to how many you can use.

``` .language-yaml
routes:
  /knock-knock-jokes/*/punch-line: joke
```

Any URL with `knock-knock-jokes` as the first segment, _anything_ as the second segment, and `punch-line` as the third, will match this route and be rendered with the `joke` template.

You can pass the value of the wildcard segment into the template and create a variable on the fly. Instead of `*` you can set a parameter like this:

``` .language-yaml
routes:
  /knock-knock-jokes/{setup}/punch-line: joke
```

Given a URL of [example.com/knock-knock-jokes/little-old-lady/punch-line](), your template can use `{{ setup }}` to access the value `little-old-lady`. This is more consistent, reliable, and readable than relying on segment variables and allows you to change the routes and rearrange your site structure without ever breaking a link.

These wildcard variables can contain upper and lowercase letters, numbers, and underscores.

> You can use wildcards and wildcard variables in your routes file, but you can't use both of them in the same route pattern.

### Ignoring Segments {#ignoring-segments}

You can also add a list of segments that should be completely and utterly ignored if found in your URL, like that poor nerdy kid at prom. This comes in handy if you have a form and want to redirect a user to `/success` but still use the same template, and other similar things we can't think of.

``` .language-yaml
ignore:
  - success
  - such
```

Keep in mind these ignore segments are **global**. `example.com/such/bummer` will be seen and routed as `example.com/bummer`, and `example.com/weird-band-names/such` as `example.com/weird-band-names`. These ignore values will still appear in your `{{ segment_n }}` variables.

Here's an example on how you might use this in the context of a form submission page.

```
{{ if segment_3 == "success" }}
    <h1>You have done it!</h1>
{{ else }}
    {{ partial:contact-form }}
{{ /if }}
```

## Collection Routes {#collection-routes}

By their very nature Collections don't determine their own URLs. To do so would limit their flexibility. This is where you get to decide just how their URLs are structured and where they go.

The syntax is very similar to the [Template Routes'](#template-routes), plus all of the collection's custom field variables are available, along with a few other "meta" variables. Instead of passing variables into templates, here you're passing variables into the URL. It's the same, except different.

- `slug`
- `year`
- `month`
- `day`
- `order_key`

``` .language-yaml
collections:
  blog: /blog/{year}/{month}/{slug}
  locations: /stores/{state}/{city}
```

Any time you're looping through the collection to render its data, each entry's `{{ url }}` will match to this format. For the `locations` collection, you could see URLs like the following:

- `http://example.com/stores/saratoga-springs/new-york`
- `http://example.com/stores/miami/florida`

And in the blog you might see this:

- `/blog/2015/12/christmas-already-what-the-heck`
- `/blog/2016/01/where-did-the-year-go`

## Taxonomy Routes {#taxonomy-routes}

Taxonomy Terms are handled almost exactly like Entries and you can define the routes used when rendering a Taxonomy listing or index for each Taxonomy.

The Taxonomy Term's `slug` variable is available for you to use in your patterns.

``` .language-yaml
taxonomies:
  categories: /blog/category/{slug}
  sizes: /products/sizes/{slug}
  colors: /products/colors/{slug}
```

> You can create any URL structure you like but keep in mind that the `taxonomy="true"` parameter on the Collection tag will assume the last 2 segments in your URL are the Taxonomy and Term's `slug`, respectively.

## Vanity URLs

Statamic allows you to quickly set up vanity URLs, which let you quickly forward one URL to another.

As an example of when these might be used: perhaps your site is running a promotion but you want to use a short, catchy URL on your marketing campaign. Rather than having to tell someone something like `/events/2013-log-cutting-championships-afterparty`, you could create a `/party` vanity URL which will forward to your event detail page.

### Defining Vanity URLs

Simply add key/values of the redirects.

```
vanity:
  /party: /my-long/page-name
```

This will forward `http://yoursite.com/party` to `http://yoursite.com/my-long/page-name`. Simple.

### Best Practices

While it’s possible to create permanent redirects with the vanity URLs feature, it’s best practice to do this via an `.htaccess` file or similar method. Vanity URL redirects are going to be slower than server-level redirects (although the difference may not be noticeable in all situations).
