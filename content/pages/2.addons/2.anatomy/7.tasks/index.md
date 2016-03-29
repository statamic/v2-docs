---
overview: >
  Tasks let you run commands at specified
  intervals.
title: Tasks
id: 4ccdc432-2a6a-4a74-9228-f09f93c9d1ad
---
Typically, to automate tasks you would use various "cron jobs" that runs things at specified intervals. You'd need to
use the command line and remember strange syntaxes to get it working. How annoying. The Task Scheduler makes this
simple.

## Using tasks

To enable task scheduling, you will need to add the following to your server's crontab:

``` .language-bash
* * * * * php /path/to/please schedule:run >> /dev/null 2>&1
```

Weird, right? Thankfully, you just have to do that once for the whole Statamic installation.
(You can just copy paste it and modify the `/path/to/` part). Otherwise, it's just that. The rest
is handled in the addon's code.

## Creating your task schedule

You will need a tasks class file in your addon directory. There's an example below. Your class should
be named `[AddonName]Tasks.php`. eg. `MyAddonTasks.php`.

You can also use `php please make:tasks AddonName` to have a class file generated for you.

The tasks file consists of one method, `schedule`, with one argument, `$schedule`.
This method is where you define your – you guessed it – schedule.

Defining your schedule in a Statamic addon is no different from Laravel, so you should consult the
[Laravel docs][laravel-scheduling] for more details.

### Example

Here's an example from a fictional `CacheCleaner` addon. It's saying: once a week, run the `cache:clear` command.

``` .language-php
<?php

namespace Statamic\Addons\CacheCleaner;

use Statamic\Extend\Tasks;
use Illuminate\Console\Scheduling\Schedule;

class CacheCleanerTasks extends Tasks
{
    /**
     * Define the task schedule
     *
     * @param \Illuminate\Console\Scheduling\Schedule $schedule
     */
    public function schedule(Schedule $schedule)
    {
        $schedule->command('cache:clear')->weekly();
    }
}
```

If you haven't already, you should [learn how to create a command][commands].

[commands]: /addons/anatomy/commands
[laravel-scheduling]: http://laravel.com/docs/5.1/scheduling#defining-schedules
