---
title: SearchQueryPerformed
class: Statamic\Events\SearchQueryPerformed
id: c71c535a-a549-4457-ac3c-e95bd13fe54f
---
Fired when a search is performed.

```
public $events = [SearchQueryPerformed::class => 'handle'];

public function handle(SearchQueryPerformed $event)
{
    $event->query; // the string that was searched for
}
```
