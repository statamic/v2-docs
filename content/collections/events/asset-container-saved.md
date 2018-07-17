---
title: AssetContainerSaved
class: Statamic\Events\Data\AssetContainerSaved
id: 2f2863f2-d32f-42d2-bd95-ed53760bfa1c
---
This is a [data event](/addons/events#data-events) that is dispatched when an asset container has been saved.

```
public function handle(AssetContainerSaved $event)
{
    // In addition to data event methods, you get:
    $event->container; // The asset container object.
}
```
