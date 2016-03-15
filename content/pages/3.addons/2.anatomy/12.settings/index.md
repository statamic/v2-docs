---
overview: >
  Addons can be configured using a number of settings. While these are stored in
  a YAML file, it's simple to create a UI for editing these through the Control Panel.
title: Settings
id: 14266569-ed5c-4804-b36c-fc66e502491e
---
## Overview

Instead of hard-coding values into your addon, you may allow them to be user-configurable. You may leverage this
using configuration settings.

``` .language-php
// Instead of hardcoding values...
$message = 'Hello John!';

// Use a config setting!
$name = $this->getConfig('greeting_name', 'John');
$message = "Hello {$name}!";
```

You should add all your settings to `default.yaml` in your addon folder. As the filename suggests, these will
act as the default values.

If a user wants to override a setting they may create a `site/addons/addon_name.yaml` file and add the value
in there. Note that the whole `default.yaml` doesn't need to be copied. Only the values that they wish to
override need to be in there.

## Creating a Settings UI {#ui}

A nice alternative to opening YAML files is the ability to edit settings through the Control Panel.

To allow your users to edit your configuration settings, simply create a `settings.yaml` fieldset
in your addon's folder.

``` .language-yaml
fields:
  foo:
    type: text
```

You should _not_ include `display` or `instructions` values. Instead, these should be added to your addon's
`resources/lang/en/settings.php` file.

``` .language-php
<?php
return [
    'foo' => 'Foo',
    'foo_instruct' => "What's a foo? You tell me."
];
```

Of course, these strings may be translated into other languages by using separate files.

## Retrieving config settings

You can use `$this->getConfig($value, $fallback)` (as well as the type-casting alternatives: `getConfigInt`,
`getConfigBool`, etc) to retrieve a value.

Config settings can be used from within any addon aspect.
