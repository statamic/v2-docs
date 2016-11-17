---
title: API Reference
id: 0bf69591-dd53-4bde-9bd7-0683ce5623de
template: api
overview: |
  Within your addons you will be able to access and manipulate content, data, and whatever else you can think of. In here, we explain all the classes you might care about and how to use them.
---
Generally speaking, you should use the classes in the `Statamic\API` namespace as entry points to the API.

However, methods in those classes, as well as other areas may return classes outside of the `Statamic\API` namespace. You will usually not need to create or instantiate these directly.

For example, want to create an entry? You will use `Statamic\API\Entry::create()`, which returns an `Statamic\Data\Entries\EntryFactory`, which will eventually return
a `Statamic\Data\Entries\Entry`. You wouldn't do `new Entry`.