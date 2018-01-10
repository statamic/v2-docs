---
title: Best Practices
overview: >
  Sometimes it's very possible that your addon may perform the same function from
  multiple points. For example, you might want to do something inside a tag, and
  from the API, and from your fieldtype, etc.
id: 7aeb656d-bf60-4509-8817-570797b02279
---
## Keeping Dry {#dry}

Writing the same thing in multiple places goes against the concept of keeping your code DRY (don't repeat yourself).

Here's a suggestion on how to organize your "core" functionality.

### The Karma example

Let's take our fictional "Karma" addon that lets you assign points to users.

We want to be able to output a user's points from a tag, as well as allowing other addons to get a user's points.
With this in mind, we can see that retrieving points will be a duplicated effort. We'll want to move that somewhere reusable.

Here's our addon directory:

``` .language-files
Karma/
|-- KarmaAPI.php
|-- KarmaTags.php
└── Scorekeeper.php
```

Here we have our API and event listener classes, as well as our own Scorekeeper class that'll – you guessed it – keep score.

Now here's the API:

``` .language-php
<?php

namespace Statamic\Addons\Karma;

use Statamic\Extend\API;

class KarmaAPI extends API
{
    private $scorekeeper;

    protected function __construct(ScoreKeeper)
    {
        $this->scorekeeper = $scorekeeper;
    }

    public function getPoints($user)
    {
        return $this->scorekeeper->getPoints($user);
    }
}
```

And the tags:

``` .language-php
<?php

namespace Statamic\Addons\Karma;

use Statamic\Extend\Tags;

class KarmaTags extends Tags
{
    private $scorekeeper;

    protected function __construct(ScoreKeeper)
    {
        $this->scorekeeper = $scorekeeper;
    }

    public function getPoints($user)
    {
        return $this->scorekeeper->getPoints($user);
    }
}
```

Finally, the scorekeeper class:

``` .language-php
<?php

namespace Statamic\Addons\Karma;

use Statamic\API\User;

class ScoreKeeper
{
    public function getPoints($user)
    {
        return User::get($user)['points'];
    }
}
```

As you can see, both addon aspects simply pass the work onto a common class.

If you'd like to use addon helper methods in your extra classes (eg. the `ScoreKeeper` class), you can use the `Statamic\Extend\Extensible` trait.
