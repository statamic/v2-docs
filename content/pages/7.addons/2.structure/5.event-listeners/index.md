---
title: Event Listeners
overview: >
  Certain, specific actions in Statamic and other addons will fire events that can be listening for, allowing for other actions to be scripted reactively That's just a fancy way of saying "When that happens, do this."
id: a5a4089e-4555-47e1-800a-e8e324cef143
---
## How Event Listeners Work

Listeners should define a list of events mapped to the class methods. When matching events are fired, the appropriate methods are executed. That list lives in the class array variable `$events`.

## Generating an Event Listener Class {#generating}

You can generate a an event listener class file with a console command.

``` .language-console
php please make:listener AddonName
```

## Example Listener

Perhaps you'd like to send a tweet after an entry has been published in the Control Panel. You could create a tweet addon that would listen for the `publish.create` event and do the deed.

``` .language-php
<?php

namespace Statamic\Addons\Tweeter;

use Statamic\Extend\Listener

class TweeterListener extends Listener
{
    public $events = [
        'publish.create' => 'sendTweet'
    ];

    public function sendTweet($entry)
    {
        // send a tweet
    }
}
```

You may also listen to multiple events and even run multiple methods for a single event. To run multiple methods for one event, simply pass an array.

``` .language-php
public $events = [
    'publish.create' => ['thisMethod', 'thatMethod']
];
```

## Returning Data

Some events will be expecting you to send back data.

For example, the control panel will send an Entry object with its `publish.create` method. Any addon listening to the event will be able to modify it, and then return it back up the chain so the Control Panel can continue its task. Be a good citizen. Return your data.

``` .language-php
public function makeEntryAwesome($entry)
{
  $entry['awesome'] = true;

  return $entry;
}
```

## Native Events {#events}

See all the [native events fired by Statamic][events].

[events]: /addons/events
