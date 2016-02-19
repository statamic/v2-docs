---
title: Modifiers
id: df6aaa3c-2ffd-4a84-be3a-1fbde5037b77
overview: >
  Modifers give you the ability to manipulate the data of your variables on the fly. They can manipulate strings, filter arrays and lists, help you compare things, do basic math, simplify your markup, and even help you debug.
---
## Just one, thanks
An addon can have _one_ modifier. This is because the modifier will take on the name of the addon.

For example, have a `Bacon` addon? If you were to include a modifier, you'd use `{{ var | bacon }}` in your templates.

You can generate a modifier file by using `php please make:modifier`.

## An example

Let's say we need a modifier that repeats things.

The Modifier class has one method: `index()`. When your modifier is used in a template, this is what will be called.

The first argument will be the `$value` we want to modify. All we need to do is return the value after we've modified it.
Once it's returned, it'll be sent back to the template where it can either be rendered or sent to the next modifier
in the chain.

The other arguments are optional - you only need to worry about them if you plan to use them:

 - `$params` will be an array of any parameters.
 - `$context` will be an array of contextual values. These are the values available at that position in the template.

``` .language-php
<?php

namespace Statamic\Addons\Repeat;

use Statamic\Extend\Modifier;

class RepeatModifier extends Modifier
{
    public function index($value, $params, $context)
    {
        // Repeat twice by default
        $repeat = 2;

        // Get the parameter, if there is one
        if ($param = array_get($params, 0)) {
            // Either get the variable from the context, or if it doesn't exist,
            // use the parameter itself - we'll assume its a number.
            $repeat = array_get($context, $param, $param);
        }

        // Repeat!
        return str_repeat($value, $repeat);
    }
}
```

``` .language-yaml
times: 5
thing: Bacon
```

```
<!-- Pipe syntax -->
{{ thing | repeat }}
{{ thing | repeat:3 }}
{{ thing | repeat:times }}

<!-- Parameter syntax -->
{{ thing repeat="true" }}
{{ thing repeat="3" }}
{{ thing repeat="times" }}
```

``` .language-output
BaconBacon
BaconBaconBacon
BaconBaconBaconBaconBacon

BaconBacon
BaconBaconBacon
BaconBaconBaconBaconBacon
```
