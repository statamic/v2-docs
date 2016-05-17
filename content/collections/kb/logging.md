---
title: How to configure and use logging
alias:
  - How do I log errors to Slack?
  - What are my logging options?
id: b518fa3d-b41f-4238-b1de-02ada96e2713
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
---
Things go wrong. It's a part of life. Fortunately, we can see what went south using logs. There are a number of different logging methods available to you in Statamic.

Statamic comes pre-configured with error logging provided by Laravel and the powerful Monolog logging library. This allows you to use a number of logging handlers, should you wish to integrate with something more advanced.

## Configuring Handlers

Your logging configuration can be edited in the Debug settings section of the CP, or by editing the `loggers` key in `debug.yaml`.

You may configure multiple handlers at the same time by adding each one with their separate key.

```
loggers:
  file: true
  slack:
    token: ...
```

All handlers accept a `level` key, where you can specify the minimum severity level that should be logged for that handler. Here they are, from least to most severe: `debug`, `info`, `notice`, `warning`, `error`, `critical`, `alert`, `emergency`.

### File

Out of the box, Statamic uses the `file` log handler. This will write to `local/storage/logs/statamic.log`.

You may customize the location of your log file by adding a `path` to the directory of your choice.

```
loggers:
  file:
    path: /home/logs
```

If you find your log file growing too large, you can enable daily files. This will create files
with datestamps. To do that, simply add `daily: true` to your `file` logger config.

```
loggers:
  file:
    daily: true
```

### Browser

Logs directly to your browser's console.

```
loggers:
  browser: true
```

### Email

Sends an email using PHP's mail() function.

```
loggers:
  email:
    to: you@example.com
    from: me@example.com
    subject: An error has occurred
```

### Hipchat

Sends logs to a [Hipchat](https://www.hipchat.com/) room.

```
loggers:
  hipchat:
    token:
    room:
    name:
```

### Slack

Sends logs to a [Slack](https://slack.com/) channel.

```
loggers:
  slack:
    token:
    channel:
    username:
    icon_emoji:
```

### Fleep

Sends logs to [Fleep.io](https://fleep.io/)

```
loggers:
  fleep:
    token:
```

### Flowdock

Sends logs to [Flowdock](https://www.flowdock.com/).

```
loggers:
  flowdock:
    token:
```

## Triggering Logs

Within code, you may trigger a log to be written (or sent, etc) by using Laravel's `Log` facade.

The logger provides the eight logging levels defined in RFC 5424: emergency, alert, critical, error, warning, notice, info and debug.

``` .language-php
Log::emergency($error);
Log::alert($error);
Log::critical($error);
Log::error($error);
Log::warning($error);
Log::notice($error);
Log::info($error);
Log::debug($error);
```

An array of contextual data may also be passed to the log methods. This contextual data will be formatted and displayed with the log message:

``` .language-php
Log::info('User failed to login.', ['id' => $user->id]);
```

As the facade belongs to the global namespace, remember to either `use Log` once, or do `\Log::info()`.
