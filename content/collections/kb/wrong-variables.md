---
title: The wrong variables are being shown
kb_categories:
  - Troubleshooting Common Scenarios
overview: Looping over some data and the wrong variables are being displayed? Perhaps ones from the current page?
id: 0400d150-134a-465e-858f-b37df6e1115f
---
You're probably running into a "feature" of [The Cascade][cascade].

The most common issue here is when you are looping over some entries, one of the entries doesn't have a certain
variable, but the "page" you are on _does_ have it. When you output that variable to your template, you expect
to see nothing, but actually get the value from the current page.

## For example...

Let's say you're viewing a post about the movie _Lord of the Rings_. On your post template you also loop over the
latest 2 movie entries:

``` .language-yaml
title: Lord of the Rings
rating: 5 stars
```

``` .language-yaml
title: Godfather
```

``` .language-yaml
title: Stepbrothers
rating: 3
```

```
<h1>{{ title }}</h1>
Rating: {{ rating }}

<h2>Other movies</h2>
{{ collection:movies limit="2" }}
    {{ title }}, {{ if rating }}Rating: {{ rating }}{{ else }}Unrated{{ /if }}
{{ /collection:movies }}
```

You might expect this to output something like this:

```
<h1>Lord of the Rings</h1>
Rating: 5

<h2>Other movies</h2>
Godfather, Unrated
Stepbrothers, Rating: 3
```

But in fact, you will see this:

```
<h1>Lord of the Rings</h1>
Rating: 5

<h2>Other movies</h2>
Godfather, Rating: 5
Stepbrothers, Rating: 3
```

## But why?

Why is the Godfather rated 5? This is because the `rating` variable is cascading down from the page (Lord of the Rings)
scope, which has `rating: 5`.

If the Godfather had a `rating`, it would override the `rating` in the cascade.

Most of the time, this is a great feature, and is what allows Statamic to have page variables available you to you
without needing a special tag to retrieve them. In this case, we don't want that.

## Then how?

The solution is to [bypass the cascade](/cascade#bypassing-the-cascade) by specifying a scope.

```
<h1>{{ title }}</h1>
Rating: {{ rating }}

<h2>Other movies</h2>
{{ collection:movies limit="2" scope="post" }}
    {{ post:title }}, {{ if post:rating }}Rating: {{ post:rating }}{{ else }}Unrated{{ /if }}
{{ /collection:movies }}
```

Here we scoped the variables in the loop under `post`. Now since we are accessing the variables specifically in the `post`
array, the cascade won't affect it.

[cascade]: /cascade
