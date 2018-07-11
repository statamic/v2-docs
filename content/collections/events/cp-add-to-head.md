---
title: cp.add_to_head
class: cp.add_to_head
id: 356340bd-b4bc-4f61-a699-805f6194a0ac
---
Fired on every CP request. Any strings returned will be added to the Control Panel’s `<head>` element. Useful for adding early-loaded Javascript.

```
public $events = ['cp.add_to_head' => 'handle'];

public function handle();
```
