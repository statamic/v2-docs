---
overview: >
  Widgets let you display anything you like on the Control Panel dashboard. From important sales data
  to a random Chuck Norris joke of the day.
title: Widgets
id: c6a4207b-8477-48db-9a7f-576b9a714031
---
## Creating a Widget

To create a widget, you will need a class file named `[AddonName]Widget.php`. eg. `ComplimentWidget.php`.

You can also use `php please make:widget AddonName` to have one generated for you.

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

## Adding the widget to the dashboard

In your Control Panel settings (`Configure > Settings > Control Panel` or `site/settings/cp.yaml`), add your widget
to the array.

```
widgets:
  -
    type: compliment
```

## Configuring a widget

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
