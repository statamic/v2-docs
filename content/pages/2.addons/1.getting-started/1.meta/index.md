---
title: Metadata
id: c4426cb1-67b6-4a03-af7d-b4564545a97c
overview: |
  What's it called? Who made it? What does it do? Statamic wants to know all about your addon.
---
The first step in building an addon is to create a metadata file. This tells Statamic about your addon and will display it in the addon manager.

Simply create a `meta.yaml` file in the root of your addon folder, and swap out our fictional addon details below with your own.

``` .language-yaml
name: Charging Bison
version: 1.0
description: Charge your customers with the grace of a Bison.
addon_url: http://statamic.com/addons/charging-bison
developer: Statamic
developer_url: http://statamic.com
```

Congratulations, you've built an addon!

Well, kinda. But it does nothing. That's like saying breakfast is ready but all you've done is set the table.
Now you'll need to cook the bacon – er, [write some code][code-structure].

[code-structure]: /addons/getting-started/code-structure