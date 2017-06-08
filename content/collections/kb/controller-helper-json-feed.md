---
title: How to use a controller to create a JSON feed
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
id: d206dffa-19ca-4c7e-b1b2-a29d050d18ce
---
Rather than trying to build a JSON object in your Statamic template, which would result in a giant mess of curly braces, you can instead use a [controller route](/routing) and a [controller in your site helpers directory](/addons/site-helpers#controllers).

In your routes file, add a route for accessing your feed, and point it at the controller:

``` .lang-yaml
routes:
  /feed.json: FeedController@json
```

The controller referenced in that route will be `Statamic\SiteHelpers\FeedController`, which can be generated with the following command:

``` .lang-bash
php please make:controller-helper feed
```

Then, the controller's `json` method can output an array.

``` .lang-php
<?php

namespace Statamic\SiteHelpers;

use Statamic\API\Entry;
use Statamic\Extend\Controller;

class FeedController extends Controller
{
    public function json()
    {
        return [
            'version' => 'https://jsonfeed.org/version/1',
            'title' => 'My Awesome Site',
            'home_page_url' => 'https://my-awesome-site.dev/',
            'feed_url' => 'https://my-awesome-site.dev/feed.json',
            'items' => $this->getItems()
        ];
    }

    private function getItems()
    {
        return Entry::whereCollection('blog')->map(function ($entry) {
            return [
                'id' => $url = $entry->url(),
                'url' => $url,
                'content_html' => markdown($entry->content()),
            ];
        })->all();
    }
}
```

If your controller returns an array, Laravel will know to convert it to a JSON response automatically.

Of course, this is just an example. You can customize and tweak this to your heart's content.
