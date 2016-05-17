---
title: Performing redirects
id: e98074ca-de64-4800-9494-aaaae55a92a9
---
## Front-matter

You can perform a redirect from any page, entry, etc, by adding a `redirect` variable to the front-matter.

For example, our page named `site/pages/bacon-of-the-week/index.md`, you might have the following:

``` .language-yaml
---
title: Bacon of the Week
redirect: /bacon/applewood
---
```

Now, when you visit `/bacon-of-the-week`, you will be taken to `/bacon/applewood`.

## Route data

The same style of redirect can be added directly to your routes:

``` .language-yaml
routes:
  /bacon-of-the-week:
    redirect: /bacon/applewood
```

## Templates

Sometimes it makes sense to perform a redirect within a template. Perhaps you have some conditional logic for
performing the redirect.

For that, you may use the aptly named [Redirect][redirect_tag] Tag.

```
{{ if should_redirect_for_whatever_reason }}
  {{ redirect to="/bacon/applewood" }}
{{ /if }}
```

## Vanity Routes

You may set Vanity Routes from within your routes file, under the `vanity` key.

``` .language-yaml
vanity:
  /bacon-of-the-week: /bacon/applewood
```

## .htaccess

This isn't really part of Statamic at all, but it will yield the most zippy results since the server would know
to perform the redirect before Statamic even gets loaded.

This is the preferred method for permanent redirects.

[redirect_tag]: /reference/tags/redirect
