---
overview: >
  Tags are one of the building blocks of
  the Statamic templating language. While
  variables can just output data, addon
  tags can perform complex functionality
  _then_ output it.
title: Tags
id: 405d7c97-7cba-4613-8653-325c8cf09812
---
A tag can be made up of several parts. Take this one, for example:

```
{{ theme:partial src="foo" }}
```

* The first part, (`theme`) is the addon's name.
* The second, (`partial`) is the method.
* Lastly, `src="foo"` is a parameter. There can be multiple parameters on a tag.

Tags can also be in pairs. For example:

```
{{ entries:listing folder="blog" }}
  <div>{{ title }}</div>
{{ /entries:listing }}
```

* The content between the tags is appropriately named: content.


## Creating tags

An addon can have multiple tags, however they are all contained in a single class.

You need a class named `[AddonName]Tags.php`. eg. `HelloTags.php`.

You can also type `php please make:tags AddonName` to have a class generated for you.

Template tags use snake_case, and will be converted to camelCase for use in your Tag class. The second segment
of a tag will be mapped to a method in your class.

For example, `{{ my_addon:my_method }}` would call `myMethod()` in the `MyAddonTags.php` class.


## Getting input

Tags, like all addon aspects, have access to the addon's configuration with the `$this->getConfig()` methods.
Configuration files are a great place to store default values, especially for values that a user may want
to change site-wide.

However, tags also have parameters (mentioned above) which are essentially used for configuration on the tag-level.
For example, for the `{{ entries:listing }}` tag, you'll usually supply a `folder` parameter to tell the tag where
to look. Your tag can access these parameter values with the `$this->getParam()` methods.

* `$this->getParam('folder')` to get a string.
* `$this->getParamBool('show_hidden')` to get a boolean.
* `$this->getParamInt('offset')` to get an integer.

There are also equivalent methods that will retrieve values from parameter _or_ config (in that order).

* `$this->get('folder')`
* `$this->getBool('show_hidden')`
* `$this->getInt('offset')`


## Outputting Data

Outputting your tag data is a little different depending on whether you intend to have a single tag or a tag pair.

### Single tags

A single tag stands alone by itself and doesn’t require a closing tag. Your tag method should return a string if you plan to display something. Within a template, your tag will be replaced with that returned string.

You may also return a boolean. This would be useful if you are intending your tag to be placed inside a conditional statement.

Naturally, if you don't return anything, your tag won't output anything. This can be useful if your tag is meant to perform
some sort of non-output task. For example, the `redirect` tag doesn't output anything, it just performs a redirect.

### Tag pairs

There are a few options for returning output with tag pairs.

#### Returning arrays

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

#### Parsing content manually

If you need to parse the content between your tag pairs, there is a helper method available to you.

The `$this->parse($data)` method accepts an associative array (like the one mentioned previously), and will return a
string that replaces any `{{ variable }}` with its appropriate values.

This method should be used when you only need one pass of the content between the tag pair.

#### Looping and parsing content

If you need to iterate over the content between the tags, there is a similar helper method available.

The `$this->parseLoop($data)` method accepts an array of associative arrays. It will loop over each array and parse
them like the `$this->parse()` would.

Your `$data` argument should look something like this:

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
