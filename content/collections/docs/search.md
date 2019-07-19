---
title: Search
id: dd2f8565-e340-426b-91f8-b11ca42f6970
overview: >
  Whether you're searching for Bobby Fischer or finding Nemo, search is a common staple of the web experience. Who has time for clicking? Get searching!
---

## Overview {#overview}

Statamic's search engine is preconfigured to Just Work™ right out of the box. All you need to do is kick off the first pass of the indexer. Which leads us to our logical first section...

## Indexes

Statamic builds indexes of your content to provide fast, relevant search results.

The default index contains all collections and taxonomies and only searches the title field of each.

### Customizing the index

You can customize which fields are indexed with the `searchable` array in `site/settings/search.yaml`.

``` .lang-yaml
searchable:
  - title
  - content
```

This setting only applies to the `default` index.

### Creating additional indexes

Each collection can have its own index. Set a `searchable` array in the collection's `folder.yaml` with a list of the fields that should be indexed.

For example, in `site/content/collections/blog/folder.yaml`:

``` .lang-yaml
searchable:
  - title
  - subtitle
  - content
```

Then run the following command to create the index:

``` .lang-bash
php please search:update collections/blog
```

### Running and updating the indexes

The indexing process can be intensive and needs to be run manually to get it started. You can run the indexer with the command `php please search:update` in your terminal, by visiting `/cp/search/update`, or simply perform a search _in the control panel_.

The Control Panel also has a setting to enable **automatic indexing** when content changes are detected.


## The Search Form {#search-form}

There’s no special search form tag that you need to use. Simply create a form that performs a GET request to where your results will be listed.

```
<form action="/search-results" method="GET">
  <input type="text" name="q" />
  <button>Search</button>
</form>
```
Assuming that the `{{ search:results }}` tag is on `/search-results`, and that you searched for `majestic stags`, submitting this form will take you to `/search-results?q=majestic+stags`.

## The Search Tag {#search-tag}

Statamic has a Search results tag that will allow you to retrieve content based on a search query. Head over to the [Search tag page][search_tag] for more details, but it works like this:

```
{{ search:results }}
	<a href="{{ url }}">{{ title }}</a>
{{ /search:results }}
```

## Drivers {#drivers}

Statamic's search component comes loaded with a couple different drivers. You may specify which to use by adding the appropriate 
`driver` value to your `site/settings/search.yaml` file.

### Local {#local-driver}

The local driver (internally named "Comb") is the default driver and it will build its index in your filesystem.

It is enabled by default, but you can be explicit about it by adding the following to your `search.yaml`:

``` .lang-yaml
driver: local
```

The local driver requires no additional setup aside from having the `local` folder writable, and from initially indexing your content.

The local driver will provide you with basic search abilities. If you are looking for more flexibility/features –
like field weighting, typo tolerance, etc – we recommend using the Algolia driver.

### Algolia {#algolia-driver}

[Algolia](https://www.algolia.com) is a popular search service. It is lightning fast and highly customizable.

To enable the Algolia driver (first make sure you have an account) head to the `Settings > Search` page, select `algolia`, and enter your API credentials.

Or, you may add the following to your `search.yaml`:

``` .lang-yaml
driver: algolia
algolia_app_id: your-app-id
algolia_api_key: your-admin-api-key
```


## Searching in the Control Panel {#searching-in-the-cp}
In the Control Panel, the search bar at the top of the page will allow you to search for content and will take you directly to the page to edit it.

When searching a collection's entries (not the global search) the [collection's index](#collection-indexes) will be favored.  
If no index exists, then the default index will be used. If the default index hasn't been created either, Statamic will simply try to find an entry with a matching title.


## Algolia {#algolia}

If you are using the Algolia driver we recommend using a Javascript implementation to communicate directly with them, as this will be incredibly fast, and avoids using Statamic as a middleman. You don't even need to worry about importing data. Statamic will handle that part when indexing.

Here's a couple of links to get you started:

- [Free Algolia Signup](https://www.algolia.com/)
- [Algolia JavaScript docs](https://www.algolia.com/doc/javascript)
- [Algolia demos](https://www.algolia.com/demos)

[search_tag]: /tags/search
