---
title: Collection
is_parent_tag: true
overview: Entries are grouped into Collections and connected to this tag to provide you means to fetch, sort, filter, and arrange them in various ways. A Collection might contain blog posts, products, or even a pile of terrible knock-knock jokes. We don't judge, and neither does the Collection Tag.
description: Grab and filter the entries in a specified Collection.
parameters:
  -
    name: from|folder|use
    type: string|array
    description: 'The name of the collection(s). Pipe separate names to fetch entries from multiple collections.'
  -
    name: collection
    type: tag part
    description: 'The name of the collection when using the shorthand syntax. This is not actually a parameter, but part of the tag itself. For example, `{{ collection:blog }}`.'
  -
    name: show_unpublished
    type: 'boolean *true*'
    description: "Unpublished content is, by it's very nature, unpublished. That is, unless you show it by turning on this parameter."
  -
    name: show_future
    type: 'boolean *false*'
    description: >
      Date-based entries from the future are
      excluded from results by default. Of
      course, if you want to show upcoming
      events or similar content, flip this
      switch.
  -
    name: show_past
    type: 'boolean *true*'
    description: >
      Just like `show_future`, but for entries
      in the past.
  -
    name: since
    type: string
    description: "Limits the date the earliest point in time from which date-based entries should be fetched. Use a plain English string here, as PHP's `strtotime` method will know what you mean. eg. `last sunday`, `january 15th, 2013`, `yesterday`."
  -
    name: until
    type: string
    description: >
      Similar to `since`, but sets the latest
      point in time.
  -
    name: sort
    type: string
    description: >
      Sort entries by field name (or `random`). You may pipe-separate multiple fields for sub-sorting and specify sort direction of each field using a colon.  
      For example, `sort="title"` or `sort="date:asc|title:desc"` to search by date then by title.
  -
    name: limit
    type: integer
    description: Limit the total results.
  -
    name: offset
    type: integer
    description: The result set will be offset by this many entries.
  -
    name: taxonomy
    type: 'boolean *false*'
    description: >
      If enabled, the current URL will be used
      filter the listing by the appropriate
      taxonomy.
  -
    name: group_by_date
    type: string
    description: >
      Allows you to group the entries by date.
      More info below.
  -
    name: filter
    type: wizardry
    description: >
      Filter the listing by either a custom
      class or using a special syntax, both of
      which are outlined in more detail below.
  -
    name: as
    type: string
    description: >
      The scope your entries can be nested
      within. More detail below.
  -
    name: paginate
    type: 'boolean *false*'
    description: >
      Specify whether your entries should be
      paginated. More detail below.
variables:
  -
    name: first
    type: boolean
    description: If this is the first item in the loop.
  -
    name: last
    type: boolean
    description: If this is the last item in the loop.
  -
    name: count|index
    type: integer
    description: >
      The number of current iteration in the
      loop.
  -
    name: zero_index
    type: integer
    description: >
      The zero-based count of the current
      iteration in the loop.
  -
    name: total_results
    type: integer
    description: The number of results in the loop.
  -
    name: page data
    type: mixed
    description: >
      Each page being iterated has access to
      all the variables inside that page. This
      includes things like `title`, `content`,
      etc.
id: 045a6e54-c792-483a-a109-f07251a79e47
---
## Example {#example}

The most basic example would be to iterate over entries in a single collection:

```
{{ collection from="blog" }}
    {{ title }}
{{ /collection }}
```

You can also use the shorthand syntax for this:

```
{{ collection:blog }}
    {{ title }}
{{ /collection:blog }}
```

If you'd like to fetch entries from multiple collections, you can only do that in the standard syntax, like so:

```
{{ collection from="blog|events" }}
    {{ title }}
{{ /collection }}
```

## Grouping entries by date {#group_by_date}

You can visually group repeating date-based entries.

When using this parameter, the templating structure you need to use will be a little different from a regular loop.

### Example

Let's assume that the entries in the `blog` collection are date-based.


```
{{ collection:blog group_by_date="M Y" as="entries" }}
    {{ date_groups }}
        <h3>{{ date_group }}</h3>
        <ul>
            {{ entries }}
                <li>{{ title }}</li>
            {{ /entries }}
        </ul>
    {{ /date_groups }}
{{ /collection:blog }}
```

The code above will output something like this:

```
<h3>May 2015</h3>
<ul>
  <li>A post from May</li>
  <li>Another from May</li>
</ul>
<h3>June 2015</h3>
<ul>
  <li>A post from June</li>
</ul>
```

The `{{ date_group }}` variable will be the date formatted by whatever you specifed in the `group_by_date` parameter.

The `{{ entries }}{{ /entries }}` tag pair will allow you to iterate over the entries in that date group. The name of this variable is specified by the `as` parameter. For example, if you used `as="posts"`, you'd use a `{{ posts }}{{ /posts }}` tag pair.

### Grouping by a custom date field {#group_by_date-custom-field}

If you'd like to group by an arbitrary date field, you can specify the field name as the second value of the parameter.

```
{{ collection:blog group_by_date="M Y|purchase_date" sort="purchase_date" }}
```

Here we are grouping on the `purchase_date` field. Note that you should also sort by that field, as the default sorting
on date-based entries would still be the entry date.


## Filtering {#filtering}

There are two options when it comes to filtering. There's the conditions syntax, and the custom filter class. You can't use both at the same time, so pick your poison.

### Conditions syntax {#conditions}

Want to get entries where the title has the words "awesome" and "thing", and "joe" is the author? You can write it how you'd say it:

```
{{ collection:blog
   title:contains="awesome"
   title:contains="thing"
   author:is="joe"
}}
```

There are a bunch of conditions available to you, like `:is`, `:isnt`, `:contains`, `:starts_with`, and `:is_before`. There are many more than that. In fact, there's a whole page dedicated to [conditions - check them out][conditions].


### Custom filters {#custom-filters}

Doing something complicated? You can reference a [custom filter][custom_filters] which can do the heavy lifting from outside of the template.

For example, want to filter drink entries by whether or not its one of the user's favorites?

```
{{ collection:drinks filter="users_favorite" }}
```

This'll load a custom filter file and do its thing from within there. Statamic makes the collection available to you, and you can manipulate it however you like. For example:

``` language-php
class UsersFavoriteFilter extends Filter
{
    public function filter()
    {
        $faves = User::getCurrent()->get('favorite_drinks');

        return $this->collection->filter(function($entry) use ($faves) {
            return in_array($entry->get('title'), $faves);
        });
    }
}
```

[Read more about custom filters][custom_filters].


## Scope {#scope}

You can optionally put your entries loop in a scope, if you’d like to be more explicit.

```
{{ collection:blog as="posts" }}
  {{ if no_results }}
    <p>No posts</p>
  {{ /if }}

  {{ posts }}
    <a href="/blog/{{ slug }}">{{ title }}</a>
  {{ /posts }}
{{ /collection:blog }}
```

You don’t have to wrap your entries loop in the `else` of your `if no_results` conditional. If there are no posts, the loop will simply output nothing.

You can [read more about scope and the cascade here](/reference/recipes/cascade).

## Pagination {#pagination}

To enable pagination mode, add the `paginate="true"` parameter, along with the `limit` parameter to specify the number of entries in each page.

```
{{ collection:blog limit="10" paginate="true" as="posts" }}

    {{ if no_results }}
        <p>Aww, there are no results.</p>
    {{ /if }}

    {{ posts }}
        <article>
            {{ title }}
        </article>
    {{ /posts }}

    {{ paginate }}
        <a href="{{ prev_page }}">⬅ Previous</a>

        {{ current_page }} of {{ total_pages }} pages
        (There are {{ total_items }} posts)

        <a href="{{ next_page }}">Next ➡</a>
    {{ /paginate }}

{{ /collection:blog }}
```

In pagination mode, your entries will be scoped (in the example, we're scoping them into the `posts` tag pair). Use that tag pair to loop over the entries in that page.

The `paginate` variable will become available to you. This is an array containing data about the paginated set.

| Variable | Description |
|----------|-------------|
| `next_page` |	The URL to the next paginated page.
| `prev_page` |	The URL to the previous paginated page.
| `total_items` |	The total number of entries.
| `total_pages` |	The number of paginated pages.
| `current_page` |	The current paginated page. (ie. the x in the ?page=x param)
| `auto_links` |	Outputs a Twitter Bootstrap ready list of links.
| `links` |	Contains data for you to construct a custom list of links.
| `links:all` |	An array of all the pages. You can loop over this and output {{ url }} and {{ page }}.
| `links:segments` |	An array of data for you to create a segmented list of links.

<br>

### Pagination Examples

The `auto_links` tag is designed to be your friend. It'll save you more than a few keystrokes, and even more headaches.
It'll output a Twitter Bootstrap-ready list of links for you. With a large number of pages, it will create segments
so that you don't end up with hundreds of numbers. You will see something like this:

![](/assets/examples/pagination-auto-links.png)

It's clever enough to work out a comfortable range of numbers to display, and it'll also throw in the prev/next arrow for good measure. Nice, right?

Maybe the Bootstrap markup isn't for you. You want something more custom. You're a maverick. That's cool. You'll want to check out the `links:all` or `links:segments` arrays. These give you all the data you need to recreate your own set of links. The `links:all` array is simply _all_ the pages with `url` and `page` variables. The `links:segments` will contain the segments like we mentioned earlier. You'll be able to access `first`, `slider`, and `last`, which are the 3 segments.

Here's the `auto_links` output, recreated using the other tags, for you mavericks out there:

```
{{ paginate }}
    <ul class="pagination">
        {{ if prev_page }}
            <li><a href="{{ prev_page }}">&laquo;</a></li>
        {{ else }}
            <li class="disabled"><span>&laquo;</span></li>
        {{ /if }}

        {{ links:segments }}

            {{ first }}
                {{ if page == current_page }}
                    <li class="active"><span>{{ page }}</span></li>
                {{ else }}
                    <li><a href="{{ url }}">{{ page }}</a></li>
                {{ /if }}
            {{ /first }}

            {{ if slider }}
                <li class="disabled"><span>...</span></li>
            {{ /if }}

            {{ slider }}
                {{ if page == current_page }}
                    <li class="active"><span>{{ page }}</span></li>
                {{ else }}
                    <li><a href="{{ url }}">{{ page }}</a></li>
                {{ /if }}
            {{ /slider }}

            {{ if slider || (!slider && last) }}
                <li class="disabled"><span>...</span></li>
            {{ /if }}

            {{ last }}
                {{ if page == current_page }}
                    <li class="active"><span>{{ page }}</span></li>
                {{ else }}
                    <li><a href="{{ url }}">{{ page }}</a></li>
                {{ /if }}
            {{ /last }}

        {{ /links:segments }}

        {{ if next_page }}
            <li><a href="{{ next_page }}">&raquo;</a></li>
        {{ else }}
            <li class="disabled"><span>&raquo;</span></li>
        {{ /if }}
    </ul>
{{ /paginate }}
```

See, just a couple of keystrokes.

[conditions]: /reference/conditions
[custom_filters]: /addons/filters
