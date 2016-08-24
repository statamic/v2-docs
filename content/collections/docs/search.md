---
title: Search
id: dd2f8565-e340-426b-91f8-b11ca42f6970
overview: >
  Whether you're searching for Bobby Fischer or finding Nemo, search is a common staple of the web experience. Who has time for clicking? Get searching!
---

## Overview

Statamic's search engine is preconfigured to Just Work™ right out of the box. All you need to do is kick off the first pass of the indexer. Which leads us to our logical first section...

## Indexing

Statamic builds indexes of your content to provide lickity-split search results. If you don't know what indexes are it doesn't matter.

To kick off the indexer you can run `php please search:update` in your terminal, visit `/cp/search/update`, or simply perform a search _in the control panel_. Also, in the Control Panel you will find settings to enable automatic indexing of content. Every time a change to content is detected a re-index occurs. If you're a nervous-saver you will probably find this setting be annoying as you sit around for a few seconds every refresh.

## Drivers

Statamic's search component comes loaded with a couple different drivers.

### Zend

Zend is the default driver and it will build its index in your filesystem. The Zend driver requires
no additional setup aside from having the `local` folder writable, and from initially indexing your content.

The Zend driver will provide you with basic search abilities. If you are looking for more flexibility/features –
like field weighting, typo tolerance, etc – we recommend using the Algolia driver.

### Algolia

[Algolia](https://algolia.com) is a popular search service. It is lightning fast and highly customizable.

To enable the Algolia driver (first make sure you have an account) head to the `Settings > Search` page, select `algolia`, and enter your API credentials. That's it.

## Searching in the Control Panel
In the Control Panel, the search bar at the top of the page will allow you to search for content and will take you directly to the page to edit it.

## Templating
Statamic has a Search results tag that will allow you to retrieve content based on a search query. Head over to the [Search tag page](/reference/tags/search) for more details.

If you are using the Algolia driver we recommend using a Javascript implementation to communicate directly with them, as this will be incredibly fast, and avoids using Statamic as a middleman. You don't even need to worry about importing data. Statamic will handle that part when indexing.

Here's a couple of links to get you started:

- [Algolia JavaScript docs](https://www.algolia.com/doc/javascript)
- [Algolia demos](https://www.algolia.com/demos)

## Regarding Bloodhound

Statamic v1's search solution, Bloodhound, had a keyword weighting feature that was pretty neat. The native Zend driver doesn't support weighting but Algolia does through their web interface. Go forth and begin searching.
