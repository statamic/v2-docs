---
overview: >
  Tasks let you run commands at specified
  intervals.
title: Tasks
id: 4ccdc432-2a6a-4a74-9228-f09f93c9d1ad
---

## Overview {#overview}

Typically, to automate tasks on a server you would use various "cron jobs" that runs things at specified intervals. You'd need to use the command line and remember strange syntaxes to get it working. How annoying. The Task Scheduler makes this simple.

## Starting the Scheduler {#starting}

To enable task scheduling, add the following Cron entry to your server:

``` .language-bash
* * * * * php /path/to/please schedule:run >> /dev/null 2>&1
```

This Cron will call the Statamic/Laravel command scheduler every minute and run any tasks that you have scheduled and runs them if they're due, automatically. This single Cron entry is all it takes.

Instead of remembering the cron syntax, you can literally copy/paste the line above (and adjust the path to the `please` file).

## Generating {#generating}

You can generate a tasks class file with a console command.

``` .language-console
php please make:tasks AddonName
```

## Defining Schedules {#defining-schedule}

The tasks class consists of one method, `schedule`, with one argument, `$schedule`. From here, it's 100% Laravel, so pop over to the [Laravel docs][laravel-scheduling] for all the juicy details.

## An Example Task {#example-task}

Here's an example from a fictional `CacheCleaner` addon that clears the cache once a week. Like a janitor, pulling pennies out of doors.

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

If you haven't already, you should learn how to [create a command][commands].

[commands]: /addons/classes/commands
[laravel-scheduling]: http://laravel.com/docs/5.1/scheduling#defining-schedules
