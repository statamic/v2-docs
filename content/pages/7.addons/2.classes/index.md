---
title: Overview
id: 395dc432-0273-4874-873e-6ed0b772cc27
---

There is no "Addon class" per-se. Generally speaking, you will create an "Addon aspect" which will have a corresponding parent class for you to extend.

For example, if you want to create a [Tag](/addons/classes/tags), you'd extend our abstract `Statamic\Extend\Tags` class.

The abstract classes will provide your class with methods and properties that you can use to access contextual information, settings, data, and other helpful things.

Depending on the aspect you're creating, you may gain additional helpers, however there are a number of them that all aspects will inherit.
You may view a list of them over at the [helper methods](/addons/helpers) section.
