---
id: d786f54b-12b7-43a8-8180-4fcd124aff0d
title: Event Reference
nav_title: Event Reference
overview: >
  Throughout the lifecycle of a Statamic request, there are a number of events that are fired. These events allow
  a developer to add or modify functionality in various places.
---

## Usage

Simpler events are provided as a string with an argument passed through as a payload. For example, the `cp.page.published` event is dispatched as a string, and passes along the `Page` that was published.

```
public $events = [
    'cp.page.published' => 'doSomething'
];

public function doSomething($entry)
{
    //
}
```

Potentially more complicated events are provided as an event class. For example, when a page is deleted, the `Statamic\Events\Data\PageDeleted` class is dispatched. This class will be provided as the argument.

```
use Statamic\Events\Data\PageDeleted;

...

public $events = [
    PageDeleted::class => 'doSomething'
];

public function doSomething(PageDeleted $event)
{
    //
}
```

## Events

### cp.add_to_head

Fired on every CP request. Any strings returned will be added to the Control Panel’s `<head>` element. Useful for adding early-loaded Javascript.

```
public $events = ['cp.add_to_head' => 'handle'];

public function handle();
```

### cache.cleared

Fired when the cache is cleared. This refers to the persistent cache, _not_ the "Stache".

```
public $events = ['cache.cleared' => 'handle'];

public function handle();
```

### content.saved

Fired when any Page, Entry, Taxonomy Term, or Global is saved. The respective class will be passed through.

```
public $events = ['content.saved' => 'handle'];

public function handle(Content $content);
```

### cp.published

Fired when any Page, Entry, Taxonomy Term, Global, or User is saved through the CP. The respective class will be passed through.

```
public $events = ['cp.published' => 'handle'];

public function handle(Data $data);
```

### cp.page.published

Fired when a page is saved through the CP. The `Page` class will be passed through.

```
public $events = ['cp.page.published' => 'handle'];

public function handle(Page $page);
```

### cp.entry.published

Fired when an entry is saved through the CP. The `Entry` class will be passed through.

```
public $events = ['cp.entry.published' => 'handle'];

public function handle(Entry $entry);
```

### cp.term.published

Fired when a taxonomy term is saved through the CP. The `Term` class will be passed through.

```
public $events = ['cp.term.published' => 'handle'];

public function handle(Term $term);
```

### cp.globals.published

Fired when a global set is saved through the CP. The `GlobalSet` class will be passed through.

```
public $events = ['cp.globals.published' => 'handle'];

public function handle(GlobalSet $global);
```

### cp.user.published

Fired when a user is saved through the CP. The `User` class will be passed through.

```
public $events = ['cp.user.published' => 'handle'];

public function handle(User $user);
```

### cp.nav.created

Fired after the navigation has been created. A `Statamic\CP\Navigation\Nav` object will be passed through.

Use this event to [add your own items to the navigation](/addons/anatomy/navigation).

```
public $events = ['cp.nav.created' => 'handle'];

public function handle(Nav $nav);
```

### Form.submission.creating

Fired when a form is submitted, but before it is saved. Allows you to stop or modify the submission.

The `Submission` object will get passed through.

To modify the submission, return an array with a `submission` key containing the modified `Submission` object.

To stop the submission and display errors, return an array with an `errors` key containing an array of error messages.

```
use Statamic\Contracts\Forms\Submission;

public $events = ['Form.submission.creating' => 'handle'];

public function handle(Submission $submission)
{
    return [
        'submission' => $submission,
        'errors' => []
    ]
}
```

### Form.submission.created

Fired when the submission is successfully saved. The `Submission` object is passed through.

```
use Statamic\Contracts\Forms\Submission;

public $events = ['Form.submission.created' => 'handle'];

public function handle(Submission $submission);
```

### glide.generated

Fired after Glide has generated an image. The parameters will be `$path` - which will be the full path to the cached image, and `$params` - an array of query parameters used to generate the image.

```
public $events = ['glide.generated' => 'handle'];

public function handle($path, $params);
```

### response.created

Fired just after the response has been created for front-end requests, and just before it gets sent.

This is a perfect time for you to modify the response by adding headers, adjusting the output, etc.

An `Illuminate\Http\Response` instance will be passed through. Simply modify it and don't return anything.

```
use Illuminate\Http\Response;

public $events = ['response.created' => 'handle'];

public function handle(Response $response);
```

### system.updated

Fired when the Updater has completed.

```
public $events = ['system.updated' => 'handle'];

public function handle(Submission $submission);
```

### user.registered

Fired after a user has registered with the [Register Form](/reference/tags/user-register_form). The `User` will be passed through.

```
public $events = ['user.registered' => 'handle'];

public function handle(User $user);
```


### Statamic\Events\StacheUpdated

Fired when the Stache is updated (ie. when content, assets, or users are created/modified) this will be fired.

The `Statamic\Events\StacheUpdated` class will be passed through.

```
use Statamic\Events\StacheUpdated;

public $events = [StacheUpdated::class => 'handle'];

public function handle(StacheUpdated $event)
{
    $event->stache; // Statamic\Contracts\Stache\Cache
    $event->updates; // \Illuminate\Support\Collection of repos that were updated
    $event->updated($repo); // Check if a given repo was updated
    $event->updatedAny($repos); // Check if any given repos were updated
}
```
