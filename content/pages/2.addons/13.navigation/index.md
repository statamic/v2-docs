---
overview: >
  Creating pages in the control panel using a [Controller](/addons/anatomy/controllers) is all well and good, but wouldn't
  it be nice if there was an easy way to access them? The Control Panel's navigation bar isn't just for core items. You
  are able to add your own items in there, too.
title: Navigation
id: a5e391e7-31ac-40a2-8f13-1746b660197b
---
## Overview

Statamic emits a `cp.nav.created` just after the Control Panel's navigation gets created. This event passes along
the `Statamic\CP\Navigation\Nav` singleton. Any items added to this class will be reflected in the navigation bar.

## Adding Items
Adding a navigation item should be done in your [Listener](/addons/anatomy/listeners) file.

Let's assume we're creating a Store addon, which has products and orders. We'll want to create a navigation item
like this:

![](/assets/examples/navigation.png)

To do that, we'll add the following code to our listener:

``` .language-php
<?php

namespace Statamic\Addons\Store;

use Statamic\API\Nav;
use Statamic\Extend\Listener;

class StoreListener extends Listener
{
    public $events = [
        'cp.nav.created' => 'addNavItems'
    ];

    public function addNavItems($nav)
    {
        // Create the first level navigation item
        $store = Nav::item('Store')->route('store')->icon('shopping-cart');

        // Add second level navigation items to it
        $store->add(function ($item) {
            $item->add(Nav::item('Products')->route('store.products'));
            $item->add(Nav::item('Orders')->route('store.orders'));
        });

        // Finally, add our first level navigation item
        // to the navigation under the 'tools' section.
        $nav->addTo('tools', $store);
    }
}
```

## The NavItem class

Each item you see in the navigation is an instance of the `Statamic\CP\Navigation\NavItem` class. Each instance may
contain its own collection of `NavItem` objects, and so on, letting you create nested sets of navigation.

### Basic API

In the code example, you'll notice `Nav::item()`. This is simply a shortcut to creating a new instance of `Statamic\CP\Navigation\NavItem`.

So, fire up a `Nav::item('foo')` then go ahead chaining with methods below:

| Method | Description |
|--------|-------------|
| `name` | The key used in the navigation tree. |
| `title` | The label to be shown in the navigation. If left blank it will title-case the `name`. |
| `url` | The `href` of the link to be rendered. |
| `route` | A shorthand to set the `url` based on a route. |
| `icon` | The name of the icon to render. Icons are only displayed on top level nav items. |
| `add` | Add a child item to the item. You can pass a `NavItem` or a closure, like the code example above. |

### Icons

The Statamic CP uses Entypo for icons. When using `$item->icon($name)`, you can pass any name available.

To see the list of icon names available, head to [the Entypo website](http://entypo.com/) where you'll
see all the icons. Find the one you like, inspect the source and the filename will be there.
