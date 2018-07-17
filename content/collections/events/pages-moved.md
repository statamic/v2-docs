---
title: PagesMoved
class: Statamic\Events\Data\PagesMoved
id: 0e476aa7-4e41-462d-9eeb-a06df3533694
---
This is a [data event](/addons/events/#data-events) that is dispatched when pages are moved when reordering the page tree.

This event is dispatched once for the entire page tree reorder operation. The [PageMoved event](/addons/events/page-moved) 
will be dispatched for each affected page.

```
public function handle(PagesMoved $event);
```
