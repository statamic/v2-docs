---
overview: >
  Widgets let you display anything you like on the Control Panel dashboard. From important sales data
  to a random Chuck Norris joke of the day.
title: Widgets
id: c6a4207b-8477-48db-9a7f-576b9a714031
---
## Creating a Widget {#creating}

To create a widget, you will need a class file named `[AddonName]Widget.php`. eg. `ComplimentWidget.php`.

You can also generate one with a command:

``` .lang-bash
php please make:widget AddonName
```

Here's a basic widget:

``` .language-php
<?php

namespace Statamic\Addons\Compliment;

use Statamic\Extend\Widget;

class ComplimentWidget extends Widget
{
    public function html()
    {
        return '<p>You look extra handsome today.</p>';
    }
}
```

Simply add an `html` method that returns a string. That's it.

## Adding the widget to the dashboard {#adding-to-dashboard}

In your Control Panel settings (`Configure > Settings > Control Panel` or `site/settings/cp.yaml`), add your widget
to the array.

``` .lang-yaml
widgets:
  -
    type: compliment
```

## Multiple Widgets {#multiple}

Since 2.6, an addon may have more than one widget. One "primary" widget, and multiple "secondary" widgets.

### Directory Structure {#directory-structure}

You may store your widget classes either in the root directory, like so:

``` .lang-files
site/addons/Bacon
|-- BaconWidget.php
|-- BitsWidget.php
`-- meta.yaml
```

...or within a `Widgets` directory/namespace if you wish to stay more organized:

``` .lang-files
site/addons/Bacon
|-- Widgets
|   |-- BaconWidget.php
|   `-- BitsWidget.php
`-- meta.yaml
```

### Primary vs. Secondary {#primary-secondary}

An addon's primary widget will use the name of the addon.  

``` .lang-template
type: addon
```

This will correspond to the `Statamic\Addons\YourAddon\YourAddonWidget` or `Statamic\Addons\YourAddon\Widgets\YourAddonWidget` class.

Secondary widgets will be use the name of the addon and the secondary name, delimited by a dot.

``` .lang-template
type: addon.secondary
```

This will correspond to the `Statamic\Addons\YourAddon\SecondaryWidget` or `Statamic\Addons\YourAddon\Widgets\SecondaryWidget` class.

## Working with Input {#input}

Widgets, like all parts of an Addon, have access to the addon’s configuration with the `$this->getConfig()` methods. Configuration files are a great place to store default values, especially for values that a user may want to change across all widget instances.

Widgets also have parameters which are used for configuration on the instance-level. For example, in the fictional `compliment` widget above, you may want to specify the `gender` of the person receiving the compliment. Your widget can access these parameter values with the `$this->getParam()` methods.

- `$this->getParam('gender')` to get a string.
- `$this->getParamBool('enthusiastic')` to get a boolean.
- `$this->getParamInt('enthusiasm_level')` to get an integer.

There are also super methods that will retrieve values from parameter or config (in that order) if none was found.

- `$this->get('gender')`
- `$this->getBool('enthusiastic')`
- `$this->getInt('enthusiasm_level')`

Let's say we have the following in the `site/addons/compliment.yaml` file:

``` .language-yaml
gender: male
enthusiastic: true
enthusiasm_level: 2
```

and the following in the `site/settings/cp.yaml` file:


``` .language-yaml
compliment:
  type: compliment
compliment_her:
  type: compliment
  gender: female
```



``` .language-php
public function html()
{
    $adjective = ($this->get('gender') === 'male') ? 'handsome' : 'beautiful';

    $punctuation = ($this->getBool('enthusiastic'))
      ? str_repeat('!', $this->getInt('enthusiasm_level'))
      : '.';

    return '<p>You look extra ' . $adjective . ' today' . $punctuation . '</p>';
}
```

In the first widget, we'll see a male compliment since the `gender` was not specified as a parameter, and will come from the config.

In the second, we'll see a female compliment, since the `gender` was specified.

In both cases, the compliment will end with 2 exclamation points, since `enthusiastic` and `enthusiasm_level` are retrieved from the config.
