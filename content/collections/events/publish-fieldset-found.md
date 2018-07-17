---
title: PublishFieldsetFound
class: Statamic\Events\Data\PublishFieldsetFound
properties:
  - 
    name: fieldset
    type: Fieldset
    description: The `Statamic\CP\Fieldset` instance. You may modify the fieldset without needing to return it.
  - 
    name: type
    type: string
    description: The type of data object. Either "page", "entry", "term", "globals", or "user"
  - 
    name: data
    type: Data
    description: >
      An instance of `Statamic\Contracts\Data\Data`. The `Page`, `Entry`, `Term`, `GlobalSet`, or `User`
      instance. When creating (as opposed to editing), this will be `null`.
id: 52fefdf5-f8a2-43c2-98d3-43732b0cf8e6
---
This is dispatched after Statamic finds the fieldset to be used on a publish page. 

```
public function handle(PublishFieldsetFound $event);
```

You may modify the fieldset here and it will be reflected in the publish form.

An example of when this would be useful is to add a section to a fieldset in the publish page on the fly.
