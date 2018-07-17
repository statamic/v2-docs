---
title: PageMoved
class: Statamic\Events\Data\PageMoved
id: 48e9334d-be29-4980-b421-cda4b91b25a5
---
This is a [data event](/addons/events/#data-events) that is dispatched when a page is moved.

Typically, this happens when reordering through the control panel. This event is dispatched once for every page that was affected
when reordering through the control panel. There is a [PagesMoved event](/addons/events/pages-moved) that will be dispatched
once for the entire operation that contains all affected pages.

```
public function handle(PageMoved $event);
```
