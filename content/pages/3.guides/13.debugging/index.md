---
title: "Pop the Hood. Let's Debug!"
nav_title: Debugging
cover: /assets/img/trail-guides/debug.jpg
description: >
  Learn how to debug your data, errors, addons, and anything else we can think of.
overview: >
  You have a website, some data, templates, and you're not writing PHP anywhere. How on earth do you look under the hood? Know when something is broken? Fine-tune performance? Excellent question. Here come the answers.
id: 39e7b7b6-905e-42fe-b232-77461c351ba3
---

## Debug Mode

Enabling Debug Mode in `System » Settings » Debugging` in the control panel, or with `debug: true` in `site/settings/debug.yaml`, will render any errors and exceptions to the browser will a full stack trace. Some of the messages are really clear and helpful, like `Can't write such and such file or folder`, but others are a bit more cryptic.

> Debugging is visible to everyone. Think hard before enabling any form of it on a live site.

## The Debug Bar {#debug-bar}
The Debug Bar has a whole pile of useful information just waiting for you. Enable it in `System » Settings » Debugging` in the control panel, or with `debug_bar: true` in `site/settings/debug.yaml`.

Once enabled, the Debug Bar will present itself to you on the front-end of your site, fixed to the bottom of the browser. It has a number of tabs, each with its own purpose, as well as some data about your environment.

![Debug Bar](/assets/img/screenshots/debug-bar.png)

#### Messages Tab

A generic catch-all for anything might dump data into the Debug Bar like a pile of garbage. The [dump modifier][dump], for example.
#### Timeline Tab

Shows you various server side processes and how long each took to render the page. Template parsing, tag rendering, and routing, for example.

#### Exceptions Tab

Displays any caught and suppressed Exceptions.

#### Events Tab

Events are any system Events that were triggered during the loading of any given page request. Each Event can have listeners attached (in an Addon, for example) and corresponding actions run. It's just nice to know what's firing when.

#### Variables Tab

Shows you all the data readily available to the page without needing to use [Tags][tags]. Page data, global variables, system settings, and more are all available in their pre-rendered data types allowing you to see [Carbon][carbon] objects, datestamps, scopes, and other interesting things.

#### Sessions Tab

Any session variables, flash data, and previous URL visited is shown here.

#### Request Tab

Information about the given page request is here, from `content_type` and `status_code` to any `POST` data and session attributes. Very handy.

## Utility Modifiers {#modifiers}

There are a few utility modifiers that let you debug data without the need for the Debug Bar. Check these bad boys out:

- [dump][dump]
- [console_log][console_log]
- [to_json][to_json]
- [debug][debug] (okay we fibbed, this one _does_ use the Debug Bar)

[tags]: /docs/tags
[carbon]: http://carbon.nesbot.com/
[dump]: /docs/modifiers#dump
[console_log]: /docs/modifiers#console_log
[debug]: /docs/modifiers#debug
[to_json]: /docs/modifiers#to_json
