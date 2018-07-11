---
title: StacheUpdated
class: Statamic\Events\StacheUpdated
id: cfe23989-068f-46e6-9616-7a8b20697a80
---
Fired when the Stache is updated (ie. when content, assets, or users are created/modified) this will be fired.

```
public $events = [StacheUpdated::class => 'handle'];

public function handle(StacheUpdated $event)
{
    $event->stache; // Statamic\Contracts\Stache\Cache
    $event->updates; // \Illuminate\Support\Collection of repos that were updated
    $event->updated($repo); // Check if a given repo was updated
    $event->updatedAny($repos); // Check if any given repos were updated
}
```
