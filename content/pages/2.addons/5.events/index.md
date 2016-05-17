---
id: d786f54b-12b7-43a8-8180-4fcd124aff0d
template: docs/events
title: Events
overview: >
  Throughout the lifecycle of a Statamic request, there are a number of events that are fired. These events allow
  a developer to add or modify functionality in various places.
events:
  -
    name: cp.add_to_head
    description: >
      Fired on every CP request. Any strings returned will be added to the Control Panel’s `<head>` element.
      Useful for adding early-loaded Javascript.
  -
    name: stache.updated
    description: >
      Fired when the Stache is updated (ie. when content, assets, or users are created/modified) this will be fired.
      The `Statamic\Events\StacheUpdated` class will be passed through.
  -
    name: cache.cleared
    description: >
      Fired when the cache is cleared. This refers to the persistent cache, _not_ the "Stache".
  -
    name: content.saved
    description: >
      Fired when any Page, Entry, Taxonomy Term, or Global is saved. The respective class will be passed through.
  -
    name: cp.published
    description: >
      Fired when any Page, Entry, Taxonomy Term, Global, or User is saved through the CP. The respective class will be passed through.
  -
    name: cp.page.published
    description: >
      Fired when a page is saved through the CP. The `Page` class will be passed through.
  -
    name: cp.entry.published
    description: >
      Fired when an entry is saved through the CP. The `Entry` class will be passed through.
  -
    name: cp.term.published
    description: >
      Fired when a taxonomy term is saved through the CP. The `Term` class will be passed through.
  -
    name: cp.globals.published
    description: >
      Fired when a global set is saved through the CP. The `GlobalContent` class will be passed through.
  -
    name: cp.user.published
    description: >
      Fired when a user is saved through the CP. The `User` class will be passed through.
  -
    name: cp.nav.created
    description: >
      Fired after the navigation has been created. A `Statamic\CP\Navigation\Nav` object will be passed through.
      Use this event to [add your own items to the navigation](/addons/anatomy/navigation).
  -
    name: Form.submission.creating
    description: >
      Fired when a form is submitted, but before it is saved. Allows you to stop or modify the submission.
      The `Submission` object will get passed through.
      To modify the submission, return an array with a `submission` key containing the modified `Submission` object.
      To stop the submission and display errors, return an array with an `errors` key containing an array of
      error messages.
  -
    name: Form.submission.created
    description: >
      Fired when the submission is successfully saved. The `Submission` object is passed through.
  -
    name: glide.generated
    description: >
      Fired after Glide has generated an image. The parameters will be `$path` - which will be the full
      path to the cached image, and `$params` - an array of query parameters used to generate the image.
  -
    name: response.created
    description: >
      Fired just after the response has been created for front-end requests, and just before it gets sent.
      This is a perfect time for you to modify the response by adding headers, adjusting the output, etc.
      An `Illuminate\Http\Response` instance will be passed through. Simply modify it and don't return anything.
  -
    name: system.updated
    description: >
      Fired when the Updater has completed.
  -
    name: user.registered
    description: >
      Fired after a user has registered with the [Register Form](/reference/tags/user-register_form). The `User` will be passed through.
---

# Event Classes

Sometimes it's necessary for the payload in an event to be more complex than a simple string or array of parameters.
In these cases, an event class will be sent along as the payload.

## Statamic\Events\StacheUpdated

This class contains 3 boolean properties: `$content`, `$assets`, and `$users` that state whether the respective caches were
updated.

It also contains a `$stache` property which contains an `Statamic\Contracts\Stache\Cache` object.
