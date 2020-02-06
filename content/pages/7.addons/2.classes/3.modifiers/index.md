---
title: Modifiers
id: df6aaa3c-2ffd-4a84-be3a-1fbde5037b77
overview: >
  Modifers give you the ability to manipulate the data of your variables on the fly. They can manipulate strings, filter arrays and lists, help you compare things, do basic math, simplify your markup, play Numberwang, and even help you debug.
---
## Anatomy of a Modifier {#anatomy}

A modifer consists of a few parts. Let's break it down.

```
{{ title | custom_modifier:neat }}
```

- The first part? That's just a regular old variable: `title`.
- Next up, the modifier name: `custom_modifier`
- And finally, a parameter: `neat`

Parameters are used to modify the behavior of a modifier. They could be anything from an integer or boolean to a variable reference. It's up to you.

## Example Class {#example-class}

Your modifier class must extend `Statamic\Extend\Modifer`and have an `index` method. The `index` method is the entry point to your modifier.

The name of the file and class can differ depending on your situation. See [Multiple Modifiers](#multiple) below.

``` .language-php
<?php

namespace Statamic\Addons\Echo;

use Statamic\Extend\Modifier;

class EchoModifier extends Modifier
{
    public function index($value, $params, $context)
    {
        return $value . ' ' . $value;
    }
}
```

## Multiple Modifiers {#multiple}

Since 2.6, an addon may have more than one modifier. One "primary" modifier, and multiple "secondary" modifiers.

### Directory Structure {#directory-structure}

You may store your modifier classes either in the root directory, like so:

``` .lang-files
site/addons/Bacon
|-- BaconModifier.php
|-- BitsModifier.php
`-- meta.yaml
```

...or within a `Modifiers` directory/namespace if you wish to stay more organized:

``` .lang-files
site/addons/Bacon
|-- Modifiers
|   |-- BaconModifier.php
|   `-- BitsModifier.php
`-- meta.yaml
```

### Primary vs. Secondary {#primary-secondary}

An addon's primary modifier will use the name of the addon.

``` .lang-template
{{ variable | your_addon }}
```

This will correspond to the `Statamic\Addons\YourAddon\YourAddonModifier` or `Statamic\Addons\YourAddon\Modifiers\YourAddonModifier` class.

Secondary modifiers will be use the name of the addon and the secondary name, delimited by a dot.

``` .lang-template
{{ variable | your_addon.secondary }}
```

This will correspond to the `Statamic\Addons\YourAddon\SecondaryModifier` or `Statamic\Addons\YourAddon\Modifiers\SecondaryModifier` class.

## Generating {#generating}

You can generate a modifier class file with a console command.

``` .language-console
php please make:modifier AddonName
```

**Note**: This will only generate a primary modifier in the root directory.

## Class Rules & Standards {#rules}

- The modifier only looks for the `index()` method.
- If working with and manipulating dates, always return a `Carbon` instance.
- If you don't return anything, your modifier will be ignored.

## Parameters and Context {#parameters-and-context}

The first and only required argument passed into `index()` will be the `$value` that needs modifying. We can do anything to this value as long as we return it when we're done. Once returned, the template will either render it, or pass it along the next modifier in the chain.

The other two arguments are optional:

 - `$params` will be an array of any parameters.
 - `$context` will be an array of contextual data available at that position in the template.

## A More Elaborate Example {#elaborate-example}

Let's say we need a modifier that repeats things. Maybe even delicious things.

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

Given the following data:

``` .language-yaml
times: 5
thing: Bacon
```

And template:

```
{{ thing | repeat }}
{{ thing | repeat:3 }}
{{ thing | repeat:times }}
```

You would find yourself with varying amounts of bacon.

``` .language-output
BaconBacon
BaconBaconBacon
BaconBaconBaconBaconBacon

BaconBacon
BaconBaconBacon
BaconBaconBaconBaconBacon
```
