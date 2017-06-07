---
title: Building Addons
description: Learn how to build your very first Statamic addon.
overview: 'An addon is how you can extend the core functionality of Statamic. Rather than digging in and messing with core files, we’ve created a system where developers can easily build new features that are compatible with everyone’s Statamic installations. Addons can then be easily shared or sold to others to let them extend their Statamic installation.'
id: a8ef93c0-6cc2-45d5-b451-7501ecd07728
---
## Regarding an Addon's responsibility

Each Addon can be viewed as a new "feature", or group of features, for your site. It can be something simple, such as a tag that returns the current time in Klingon, or something rather large, like an Ecommerce Shopping Cart. Although the complexity varies between the examples, each solves one clear problem.

If you're in the need for a quick feature, you may consider creating a [site helper][site-helpers]. These are good place to throw a bunch of extensions without needing to go through creating a whole addon.

## Assumed Knowledge

Statamic Addons involve writing PHP. While we have a lot of helper methods and API classes to take advantage of, you'll still need to know how to write PHP, and be familiary with [object oriented programming][oop], [PHP namespacing][namespacing], and ideally - [Laravel][laravel].

## Addon Structure

An addon can contain a number of classes, each. It can be made up of one of them, or up to all of them. Each aspect accomplishes different missions.

- [Tags][tags] create tags for use within templates.
- [Modifiers][modifiers] are used in your templates to manipulate variables.
- [Fieldtypes][fieldtypes] create new ways to enter data into the Control Panel.
- [Event listeners][event-listeners] can perform functionality when specific events are executed throughout the application.
- [Commands][commands] are used for executing functionality through the terminal.
- [Tasks][tasks] allow you to schedule automated functionality at a predefined schedule.
- [Service Providers][service-providers] let you bootstrap functionality into the application.
- [API][api] allows you to interact with other addons, and them with you.
- [Controllers][controllers] allow you to create pages in the control panel.
- [Filters][filters] let you perform more complicated filtering of content on certain tags.
- [Widgets][widgets] can be added to your Control Panel's dashboard.


By using combinations of these aspects in your addons, you can create some truly fascinating results. And remember, all of these aspects are simply PHP, so anything you can do with PHP is possible here.

Not to mention, Statamic is built upon [Laravel][laravel], so you can use any of [Laravel’s features][laravels-features] if you’d like.

So, what to tackle first? Let's [get started][getting-started]..

[getting-started]: /addons/getting-started
[addon]: /addons/classes/addon
[tags]: /addons/classes/tags
[modifiers]: /addons/classes/modifiers
[fieldtypes]: /addons/classes/fieldtypes
[event-listeners]: /addons/classes/event-listeners
[commands]: /addons/classes/commands
[tasks]: /addons/classes/tasks
[service-providers]: /addons/classes/service-providers
[api]: /addons/classes/api
[controllers]: /addons/classes/controllers
[filters]: /addons/classes/filters
[widgets]: /addons/classes/widgets
[laravel]: http://laravel.com
[laravels-features]: http://laravel.com/docs
[oop]: http://php.net/manual/en/language.oop5.php
[namespacing]: http://www.phptherightway.com/#namespaces
[site-helpers]: /addons/site-helpers