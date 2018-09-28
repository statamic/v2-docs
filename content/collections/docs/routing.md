---
title: Routing
id: 96fc1a83-6fe6-48b5-a8ba-7d66ab0ab2ff
overview: |
  Routes are rules that map URL patterns to your templates, content, other URLs, or other parts of the application.
video: https://youtu.be/_to78g1XvJ8
---

All of Statamic's routes are defined in `site/settings/routes.yaml` (also accessible in the Control Panel `Settings » Routes` area). We support five different types of routes.

## Template Routes {#template-routes}

Template routes allow you to create individual URLs or patterns that map to specific templates. This is very different than using a Page to create a URL because there is no inherent content manageable in the Control Panel or content files. Instead, you can code anything you need to into the template itself, use [Tags](/tags) and [Globals](globals) to display content, or pass data in through the route itself.

This is a useful technique for creating pages that aren't editable in the Control Panel, or where an empty page might be more confusing than helpful. RSS feeds, site maps, search results, and other utility pages are a few such examples.

To create a template route set the desired URL or pattern as a key under `routes`, and then the template and any additional variables or settings you wish to pass through in a key-value array.

``` .language-yaml
/search:
  template: search-results
  title: Look What I Found!
  layout: my-layout
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

You can pass the value of the wildcard segment into the template and create a variable on the fly. Instead of `*` you can set a parameter _name_ like this:

``` .language-yaml
routes:
  /knock-knock-jokes/{setup}/punch-line: joke
```

Given a URL of [example.com/knock-knock-jokes/little-old-lady/punch-line](), your template can use `{{ setup }}` to access the value `little-old-lady`. This is more consistent, reliable, and readable than relying on segment variables and allows you to change the routes and rearrange your site structure without ever breaking a link.

These wildcard variables can contain upper and lowercase letters, numbers, and underscores.

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

### Loading Content [Since 2.8.0] {#loading-content}

You may automatically load content into routes by providing a `load` variable pointing to either a URL or an ID of a content item.

``` .lang-yaml
routes:
  biography:
    template: about
    load: /about
```

This will make all the variables from the `/about` page available as top level variables on the `/biography` route, just as though you were on the page itself. This allows you to do away with any extra `get_content` tags.


## Controller Routes {#controllers}

If you're in need of more control of your output, you may use a controller. Specifically, one located in your [site helpers](/addons/site-helpers) area.

``` .lang-yaml
routes:
  /complicated/stuff: ComplicatedController@stuff
```

This will output whatever the `stuff` method in your `Statamic\SiteHelpers\ComplicatedController` class returns.

Make sure to return something that Laravel can convert into a response.  
For example: a string, an array (which will be converted to JSON), or a `Response` object.


## Collection and Taxonomy Routes {#collection-routes}

By their very nature, Collections and Taxonomies don't determine their own URLs. To do so would limit their flexibility. This is where you get to decide just how their URLs are structured and where they go.

The syntax is very similar to the [wildcards](#wildcard-segments) above, but instead of passing variables into templates, here you're passing variables into the URL. It's the same, except different.

Here are some things to note:

- Entries and terms can use any variables from their data / front-matter.
- Date-based entries also have access to `year`, `month`, and `day`.
- Values will be slugified. Handy if the values are multi-word or contain international characters.
- If an array is encountered, the first item will be used. (More info below)

For example:

``` .language-yaml
collections:
  blog: /blog/{year}/{month}/{slug}
  locations: /stores/{state}/{city}
taxonomies:
  tags: /blog/tags/{slug}
```

Any time you're looping through the collection to render its data, each entry's `{{ url }}` will match to this format. For the `locations` collection, you could see URLs like the following:

- `http://example.com/stores/new-york/saratoga-springs`
- `http://example.com/stores/florida/miami`

And in the blog you might see this:

- `/blog/2015/12/christmas-already-what-the-heck`
- `/blog/2016/01/where-did-the-year-go`

Terms get their own URLs too. Some tag URLs might look like this:

- `/blog/tags/news`
- `/blog/tags/events`

### Arrays in Routes {#arrays-in-routes}

When a route contains an array value, Statamic will use the first item.  

We'll use a typical clothing store example. Say they have a product collection, where one product could be
in multiple categories, like this:

``` .language-yaml
title: Awesome Shirt
categories:
  - shirts
  - menswear
```

Given a route of `/products/{categories}/{slug}`, the URL would be `/products/shirts/awesome-shirt`.  
Since `categories` is an array, Statamic used the first item, which was `shirts`.


## Redirects {#redirects}

Statamic supports 2 methods of redirecting routes. "Vanity" (ie. not permanent) redirects, and regular (ie. permanent) redirects. They both have their pros and cons.

### Vanity URLs {#vanity-urls}

A Vanity URL is a dummy, easy to remember URL that redirects you to a permanent URL. A perfect use case would be to create a `example.com/promo` URL that redirects you to `example.com/blog/2016/09-this-months-promo`. You can change this redirect to any URL, any time, and never have to update your marketing material, ad buy, ad so on.

``` .language-yaml
vanity:
  /party: /my-long/page-name
```

This will forward `http://yoursite.com/party` to `http://yoursite.com/my-long/page-name`. Simple.

**Best Practices**  
Vanity routes are temporary/utility 302s and should not be used as permanent redirects. It's a Google thing.

### Regular / Permanent / 301 Redirects {#301-redirects}

No matter what you call them, these are meant to be permanent redirects.

``` .language-yaml
redirect:
  /party: /my-long/page-name
```

**Best Practices**  
Browsers cache 301 redirects agressively, so use these accordingly. Ideally 301 redirects should be handled via rules in an `.htaccess` file or nginx conf rule. Server level redirects are faster because the requests don't have to run Statamic to find where to send the request to.


## Content Types {#content-types}

You can modify the `Content-Type` request headers (not to be confused with Statamic "Content Types") from `text/html` to anything else you might need. Add `content_type` to your front-matter or route definition with the appropriate MIME type, like so:

``` .language-yaml
  content_type: application/json
```

We've also added a few shorthands you can use without needing to look anything up.

- `json` becomes: `application/json`
- `xml` becomes: `text/xml`
- `atom` becomes: `application/atom+xml` and also ensures the `charset` is `utf8`
