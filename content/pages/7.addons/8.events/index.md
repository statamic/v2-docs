---
id: d786f54b-12b7-43a8-8180-4fcd124aff0d
title: Events Reference
overview: >
  Events serve as a great way to decouple various aspects of your addon, or even modify behavior or output of core functionality. A single event can have multiple listeners that do not depend on each other.
language: php
template: events
---

## Overview {#overview}

Simpler events are provided as a string with an argument passed through as a payload. For example, the `cp.page.published` event is dispatched as a string, and passes along the `Page` that was published.

```
public $events = [
    'cp.page.published' => 'doSomething'
];

public function doSomething($entry)
{
    //
}
```

Potentially more complicated events are provided as an event class. For example, when a page is deleted, the `Statamic\Events\Data\PageDeleted` class is dispatched. This class will be provided as the argument.

```
use Statamic\Events\Data\PageDeleted;

...

public $events = [
    PageDeleted::class => 'doSomething'
];

public function doSomething(PageDeleted $event)
{
    //
}
```

## Laravel Events {#laravel}

Statamic provides its own events, however you are free to also listen for any [Laravel events](https://laravel.com/docs/5.1/events#framework-events) such as `auth.login` and `auth.logout`.

## Data Events [Since 2.10] {#data-events}

Statamic will dispatch a number of "Data Events" when things are modified throughout the system. Typically these are for things
that would affect items in the filesystem. For example, when a page is saved, deleted, or moved; or when settings are updated.

All of these events make two methods available to you:

```
public function handle($event)
{
    $event->affectedPaths();
    // An array of file paths that have been affected by the action.
    // For example:
    // [
    //     /path/to/content/pages/oldpageslug/index.md, 
    //     /path/to/content/pages/newpageslug/index.md
    // ]
    
    $event->contextualData();
    // An array representation of the item that was saved.
    // For example, the data in a page, or an array of configuration settings.
}
```

## Events
