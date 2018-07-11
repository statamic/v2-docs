---
title: cp.published
class: cp.published
id: 2a170192-f4b2-4f63-8ccd-45fd2c423002
---
Fired when any Page, Entry, Taxonomy Term, Global, or User is saved **through the Control Panel**. The respective class will be passed through.

```
public $events = ['cp.published' => 'handle'];

public function handle(Data $data);
```

If you require listening for more a more specific version, an event for the respective type will also be dispatched:

- `cp.page.published`
- `cp.entry.published`
- `cp.term.published`
- `cp.globals.published`
- `cp.user.published`
