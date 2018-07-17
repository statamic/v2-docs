---
title: FindingFieldset
class: Statamic\Events\Data\FindingFieldset
id: 386f3e48-85f9-45d3-b8a0-ac586b5879ab
---
This is dispatched when Statamic attemps to find a fieldset for a specific content object. 

```
public function handle(FindingFieldset $event)
{
    $event->data; // The content object. The Page, Entry, etc
    $event->fieldset; // The fieldset.
}
```

You may modify the fieldset here and it will be reflected in whatever was requesting it.

An example of when this would be useful is to add a section to a fieldset in the publish page on the fly.
