---
title: ContentSaved
class: Statamic\Events\Data\ContentSaved
id: fe3d1706-9469-477c-b189-bcb70592165c
---
This is a [data event](/addons/events#data-events) that is dispatched when any Page, Entry, Taxonomy Term, or Global is saved.

```
public function handle(ContentSaved $event)
{
    // In addition to data event methods, you get:
    $event->data; // The Content object. eg. Page, Entry, Term, GlobalSet.
    $event->original; // An array representation of the object before it was saved.
}
```

If you require listening for more a more specific version, an event for the respective content type will also be dispatched:

- `Statamic\Events\Data\PageSaved`
- `Statamic\Events\Data\EntrySaved`
- `Statamic\Events\Data\TermSaved`
- `Statamic\Events\Data\GlobalsSaved`
