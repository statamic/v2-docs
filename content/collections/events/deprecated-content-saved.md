---
title: content.saved
class: content.saved
deprecated: true
id: 912ac69b-83bf-4831-9083-e617520c0f3e
---
See [ContentSaved](/addons/events/content-saved)

Fired when any Page, Entry, Taxonomy Term, or Global is saved. The respective class will be passed through, along with original attributes from before it was updated.

```
public $events = ['content.saved' => 'handle'];

public function handle(Content $content, $original);
```
