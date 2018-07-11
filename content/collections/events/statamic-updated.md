---
title: StatamicUpdated
class: Statamic\Events\StatamicUpdated
id: 5c71e252-9482-4344-87fb-662dba64a5b0
---
Fired when the Updater has completed, either through the Control Panel or the command line.

```
public $events = [StatamicUpdated::class => 'handle'];

public function handle(StatamicUpdated $event)
{
    $event->version;          // The current/new version of Statamic.
    $event->previousVersion;  // The version Statamic was updated from.
}
```
