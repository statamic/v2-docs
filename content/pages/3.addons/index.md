---
title: Addon Development
cover: /assets/img/trail-guides/addon.jpg
description: >
  Learn how to build your very first
  Statamic addon.
overview: >
  An addon is how you can extend the core
  functionality of Statamic. Rather than
  digging in and messing with core files,
  we’ve created a system where
  developers can easily build new features
  that are compatible with everyone’s
  Statamic installations. Addons can then
  be easily shared or sold to others to
  let them extend their Statamic
  installation.
id: e5bb505d-2096-4e69-ad91-ed8b15c64f59
---
One addon should be thought of as one new feature for your site. A “feature” can be something simple (such as a tag that returns the current time), or something large (such as Statamic’s form-builder, that builds, validates, and can process form-data). Although the complexity varies, each of those examples tackle one issue.

An addon can contain a number of aspects. It can be made up of one of them, or up to all of them. Each aspect accomplishes different missions.

- The [Addon][addon] container itself has functionality that other aspects inherit.
- [Tags][tags] create tags for use within templates.
- [Modifiers][modifiers] are used in your templates to manipulate variables.
- [Fieldtypes][fieldtypes] create new ways to enter data into the Control Panel.
- [Event listeners][event-listeners] can perform functionality when specific events are excecuted throughout the application.
- [Commands][commands] are used for executing functionality through the terminal.
- [Tasks][tasks] allow you to schedule automated functionality at a predefined schedule.
- [Service Providers][service-providers] let you bootstrap functionality into the application.
- [API][api] allows you to interact with other addons, and them with you.
- [Controllers][controllers] allow you to create pages in the control panel.


By using combinations of these aspects in your addons, you can create some truly fascinating results. And remember, all of these aspects are simply PHP, so anything you can do with PHP is possible here.

Not to mention, Statamic is built upon [Laravel][laravel], so you can use any of [Laravel’s features][laravels-features] if you’d like.

So, what to tackle first? Let's [get started][getting-started]..

[getting-started]: /addons/getting-started
[addon]: /addons/anatomy/addon
[tags]: /addons/anatomy/tags
[modifiers]: /addons/anatomy/modifiers
[fieldtypes]: /addons/anatomy/fieldtypes
[event-listeners]: /addons/anatomy/event-listeners
[commands]: /addons/anatomy/commands
[tasks]: /addons/anatomy/tasks
[service-providers]: /addons/anatomy/service-providers
[api]: /addons/anatomy/api
[controllers]: /addons/anatomy/controllers
[laravel]: http://laravel.com
[laravels-features]: http://laravel.com/docs
