---
title: cp.nav.created
class: cp.nav.created
id: eb5217f2-41d1-4c24-ae0b-4c779f48f16a
---
Fired after the navigation has been created. A `Statamic\CP\Navigation\Nav` object will be passed through.

Use this event to [add your own items to the navigation](/addons/control-panel).

```
public $events = ['cp.nav.created' => 'handle'];

public function handle(Nav $nav);
```
