---
title: Routing
id: 96fc1a83-6fe6-48b5-a8ba-7d66ab0ab2ff
overview: |
  Routes are rules that map URL patterns to your templates, content, other URLs, or other parts of the application.
---

All of Statamic's routes are defined in `site/settings/routes.yaml` (also accessible in the Control Panel `Settings » Routes` area). We support Five different types of routes.

## Template Routes {#template-routes}

Template routes allow you to create individual URLs or patterns that map to specific templates. This is very different than using a Page to create a URL because there is no inherent content manageable in the Control Panel or content files. Instead, you can code anything you need to into the template itself, use [Tags](/tags) and [Globals](/guides/content-types/#globals) to display content, or pass data in through the route itself.

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

- `http://example.com/stores/new-york/saratoga-springs`
- `http://example.com/stores/florida/miami`

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

## Redirects {#redirects}

_document these_

### Best Practices

While it’s possible to create permanent redirects with the vanity URLs feature, it’s best practice to do this via an `.htaccess` file or similar method. Vanity URL redirects are going to be slower than server-level redirects (although the difference may not be noticeable in all situations).

## Vanity URLs {#vanity-urls}

A Vanity URL is a dummy, easy to remember URL that redirects you to a perminant URL. A perfect use case would be to create a `example.com/promo` URL that redirects you to `example.com/blog/2016/09-this-months-promo`. You can change this redirect to any URL, any time, and never have to update your marketing material, ad buy, ad so on.



```
vanity:
  /party: /my-long/page-name
```

This will forward `http://yoursite.com/party` to `http://yoursite.com/my-long/page-name`. Simple.

### Best Practices

Vanity routes are _not_ perminant redirects, but rather temporary or utility 302s. The 

## Content Types {#content-types}

By default, pages are served as `text/html` which shouldn't come as a surprise. However, you may want to serve other
types, like an RSS feed or JSON. To do this, just add `content_type` to your front-matter or route definition with the
appropriate MIME type, like so:

``` .language-yaml
  content_type: application/json
```

We've also added a few shorthands to make life a little easier:

- `json` becomes: `application/json`
- `xml` becomes: `text/xml`
- `atom` becomes: `application/atom+xml` and also ensures the `charset` is `utf8`
