---
overview: >
  At certain times, Statamic will fire off
  events. Anyone that is listening for
  those events will be able to perform
  some sort of functionality.
title: Event Listeners
id: a5a4089e-4555-47e1-800a-e8e324cef143
---
You can see all the events available to you on the [Event Reference][event-reference] page.

## An example

For example, maybe you want to tweet after an entry has been created in the control panel. You could create a tweet plugin that would listen for the `publish.create` event.

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

The main thing here to notice is the `$events` property. This is where you define what events you want to listen to, and which methods will be called when the event is fired.

You may listen to multiple events, and even run multiple methods for a single event. To run multiple methods for one event, simply use another array, like this:

``` .language-php
public $events = [
    'publish.create' => ['thisMethod', 'thatMethod']
];
```

## Returning data

Some events will be expecting you to send back some data.

For example, the control panel will send the entry along with its `publish.create` method. Anyone listening to the event will be able to modify it, but they'll also need to send the entry back so the CP can continue where it left off.

Returning data is as easy as you'd expect. Simply `return` it. For example:

``` .language-php
public function makeEntryAwesome($entry)
{
  $entry['awesome'] = true;

  return $entry;
}
```

[event-reference]: /addons/events
