---
title: Tags
overview: >
  Ultimately a Tag is nothing more than a PHP Method called from an Antlers template. This common pattern allows non-PHP developers to take advantage of dynamic features in their site easily without writing any code.
id: 405d7c97-7cba-4613-8653-325c8cf09812
---
## Anatomy of a Tag {#anatomy}

A tag consists of several parts, none of which are named the "thorax". Let's break a Tag down:

```
{{ theme:partial src="foo" }}
```

* The first part? That's the addon's name: `theme`
* The second bit is the method it maps to: `partial`
* And lastly, a parameter: `src="foo"`. There can be any number of parameters on a Tag.

Tags can also come in pairs, much like beer comes in pints. For example:

```
{{ entries:listing folder="blog" }}
  <div>{{ title }}</div>
{{ /entries:listing }}
```

Anything in-between your tag pair is available as `$this->content`. Sometimes you'll want to use it as input, other times manipulate it, and yet another time leave it be. It's up to you.


## Example Class {#example-class}

The class file must be named `AddonNameTags.php`, located in the root of your addon directory, and must extend `Statamic\Extend\Tags`.

```{.language-php}
<?php

namespace Statamic\Addons\ExampleAddon;

use Statamic\Extend\Tags;

class ExampleAddonTags extends Tags
{
    public function myMethod()
    {
        return 'This is not exciting, but it works.';
    }
}
```

## Generating a Tags Class {#generating}

You can generate a Tags class with a console command.

``` {.language-console}
php please make:tags AddonName
```

## Class Rules & Standards {#rules}

- Returning a string will render said string in your template.
- Each `public` method in your Tags class is exposed as a template Tag.
- The `index()` method maps to the single part Tag, such as `{{ addon_name }}`.
- Your class name must be in the following format: `[AddonName]Tags.php`.
- Tags are `snake_case` in your templates and `camelCase` in your class.

## Working with Input {#input}

Tags, like all parts of an Addon, have access to the addon's configuration with the `$this->getConfig()` methods. Configuration files are a great place to store default values, especially for values that a user may want to change site-wide.

Tags also have parameters which are used for configuration on the _tag-level_. For example, in the native `{{ nav }}` Tag, you'll generally supply a `folder` parameter to tell Statamic where to start fetching Pages from. Your Tag can access these parameter values with the `$this->getParam()` methods.

- `$this->getParam('folder')` to get a string.
- `$this->getParamBool('show_hidden')` to get a boolean.
- `$this->getParamInt('offset')` to get an integer.

There are also super methods that will retrieve values from parameter _or_ config (in that order) if none was found.

- `$this->get('folder')`
- `$this->getBool('show_hidden')`
- `$this->getInt('offset')`


## Rendering Data {#rendering}

Rendering your tag data is a little different depending on whether you intend to have a single tag or a tag pair.

### Single Tags {#single-tags}

A single tag stands alone by itself and does not have a closing tag. Your method must return a `string` if you to render something. Within a template, your tag will be replaced with that returned string.

You may also return a `boolean`. This is useful if your tag is designed to be used in conditions.

If your tag doesn't return anything, your tag won't render anything. This can be useful if you need to perform some sort of non-HTML rendering task. For example, the `redirect` tag doesn't output any HTML, it just performs a redirect.

### Tag Pairs {#tag-pairs}

There are a few options for returning output with tag pairs.

#### Returning Arrays {#arrays}

The first option is to return an associative array. This will map variables automatically for you.

For example, if you were to return this:

``` .language-php
return [
   'tree' => 'maple',
   'path' => 'dirt',
   'sky'  => 'blue'
];
```

The `{{ tree }}`, `{{ path }}`, and `{{ sky }}` tags will be available within your tag pair — they will be replaced
by the corresponding values. So `{{ tree }}` will become `maple`, etc.

#### Parsing Content

If you need to parse the content between your tag pairs for other tags, variables, and so on, there is a helper method available to you.

The `$this->parse($data)` method accepts an associative array (like the one mentioned previously), and will return a string that replaces any `{{ variable }}` with its appropriate values.

This method is used when your string only needs to be parsed a single time. 

#### Looping and Parsing {#looping-and-parsing}

If you need to iterate over content between the tags (for example, fetching records from a Database or remote API and listing them) there is a corresponding helper method available.

The `$this->parseLoop($data)` method accepts an array of associative arrays. It will loop over each array and parse them like the `$this->parse()` would.

Your `$data` argument may look something like this:

``` .language-php
$data = [
  [
    'tree' => 'maple',
    'path' => 'dirt',
    'sky'  => 'blue'
  ],
  [
    'tree' => 'oak',
    'path' => 'asphalt',
    'sky'  => 'overcast'
  ]
];
```
