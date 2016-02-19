---
overview: "Addons can define an API for use inside of other addons. This means that addons don't need to be their own little walled-garden, but can do complex tasks and share data with those that desire it."
title: APIs
id: fd6c9acc-4ee2-45f9-aa12-4d7acea6a5ed
---
## Creating an API

To create an API, you will need a class file named `[AddonName]API.php`. eg. `KarmaAPI.php`.

You can also use `php please make:api AddonName` to have one generated for you.

Any public methods in your API class will be accessible from other addons by using
`$this->api('AddonName')->method()`.

We recommend that you keep your API simple and just use it to call methods from your
other addon classes. It's basically just a middleman between other addons and your main
addon's functionality.

## Example

Let's say we have an addon, `Karma`, that keeps track of users' karma points. Your addon
can modify the scores and output them. You also want to allow other addons to do these
things. That's where your API comes into play.

``` .language-php
<?php

namespace Statamic\Addons\Karma;

use Statamic\Extend\API;

class KarmaAPI extends API
{
    private $scorekeeper;

    protected function init()
    {
        $this->scorekeeper = new Scorekeeper;
    }

    public function getPoints($user)
    {
        return $this->scorekeeper->getPoints($user);
    }

    public function increment($user, $points = 1)
    {
        $this->scorekeeper->increment($user, $points);
    }

    public function decrement($user, $points = 1)
    {
        $this->scorekeeper->decrement($user, $points);
    }
}
```

As you can see, this API is simply handing the work over to our fictional scorekeeper class.
In another addon, they may do `$this->api('Karma')->getPoints($user)` to get the number of
points for a particular user.

Of course, if you wanted to put your logic directly in this class, that's okay too.
