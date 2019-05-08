---
title: Filters
overview: Filters allow you to filter your collections with your methods and desires.
id: a0de327a-991a-4d22-b478-fd6ff9ae4c5f
---

The built in conditions are a handy way to filter your collections using simple English phrasing. You can say things like
"where this field contains this value". But sometimes you need more control. Filters will allow you to use your own
logic to filter the collection however you like.

## Usage

Inside a collection tag ([Collection][collection_tag], [Taxonomy][taxonomy_tag], etc) you can add `filter="my_filter"`
instead of using the conditions syntax (eg. `field:is="something"`). This will spin up your filter class and perform
the filtering for you.

You can use snake_case in your parameter for consistency - Statamic will convert it to StudlyCase for you.

You can create a new filter by adding `site/addons/MyAddon/MyAddonFilter.php`, or have one generated for you by using
 `php please make:filter MyAddon`.

## Example

Let's say you wanted to only show entries whose title contained the word "bacon". (Yes, you could do this with regular
conditions, but hey it's an example.)

``` .language-php
<?php

namespace Statamic\Addons\Word;

use Statamic\Extend\Filter;

class WordFilter extends Filter
{
    public function filter($collection)
    {
        return $collection->filter(function ($entry) {
            return str_contains($entry->get('title'), 'bacon');
        });
    }
}
```

A few things to note:

- Your class' `filter` method should return a `Collection`. This is what gets sent back to your tag.
- Although `Statamic\Extend\Filter` gives you a `$this->collection` to play with—and you may seem examples kicking around that do things that way—the use of this property was deprecated in 2.6. Instead, you should use the argument passed to the `filter()` method ("`$collection`" in the example ablove).
- Within the collection, you will get an instance of the data object. In this example we're looping through entries, so we get an `Entry` object.
- You aren't restricted to using `->filter()` on the collection. Feel free to manipulate the collection however you wish using the [methods listed on the Laravel docs][collection_methods].

### Context and Parameters

Within your filter, you have access to the context and any parameters on the tag.

Context is simple enough. It will be an array of data which you can access using `$this->context`.

For parameters, you can access these using the same methods available on [Tags][tags]. This includes things like `$this->get()`, `$this->getBool()`, etc.

Looking back at the example above, let's say you don't want to assume the word is always "bacon". You can read from parameters.

```
{{ collection:blog filter="word" word="pie" }}
```

``` .language-php
class WordFilter extends Filter
{
    public function filter($collection)
    {
        return $collection->filter(function ($entry) {
            return str_contains(
                $entry->get('title'),
                $this->get('word', 'bacon')
            );
        });
    }
}
```

Now we're getting the value of the `word` parameter, and if it doesn't exist we default back to `bacon`. Delicious, dependable bacon.


[collection_tag]: /tags/collection
[taxonomy_tag]: /tags/taxonomy
[collection_methods]: https://laravel.com/docs/5.1/collections#available-methods
[tags]: /addons/classes/tags
