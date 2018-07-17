---
title: ContentDeleted
class: Statamic\Events\Data\ContentDeleted
id: 3a226dda-2a67-41b6-904b-4471a558500b
---
This is a [data event](/addons/events#data-events) that is dispatched when any Page, Entry, Taxonomy Term, or Global is deleted.

```
public function handle(ContentDeleted $event)
{
    // In addition to data event methods, you get:
    $event->id; // The ID of the object that was deleted.
}
```

If you require listening for more a more specific version, an event for the respective content type will also be dispatched:

- `Statamic\Events\Data\PageDeleted`
- `Statamic\Events\Data\EntryDeleted`
- `Statamic\Events\Data\TermDeleted`
- `Statamic\Events\Data\GlobalsDeleted`
