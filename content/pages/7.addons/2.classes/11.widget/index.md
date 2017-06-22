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

## Configuring a widget {#configuring}

You may add configuration options to your widget, much like parameters on tags or options in a fieldtype.

Let's say we want to add the option to specify the `gender` receiving the compliment.

``` .language-yaml
widget:
  type: compliment
  gender: female
```

You can retrieve the config option within the Widget class by using `$this->getConfig('gender')`.

``` .language-php
public function html()
{
    $adjective = ($this->getConfig('gender', 'male') === 'male')
        ? 'handsome' : 'beautiful';

    return '<p>You look extra ' . $adjective . ' today.</p>';
}
```
