---
title: Widgets
id: d64230b4-2447-4fe3-bbf5-2a2d6d6a2466
overview: Widgets make your Control Panel a more friendly and useful entry point.
template: widgets
---
The dashboard of the Control Panel may contain any number of widgets. A widget can contain anything you can think of. From a list of recent entries to a randomized inspiration quote.

Statamic comes bundled with a handful of widgets, however you may also [create your own](/addons/classes/widget) or use ones created by others.

## Configuration {#configuration}

Widgets can be added to the dashboard by modifying the `widgets` array in `site/settings/cp.yaml`.

Each item in the array should specify the widget as the `type`, plus any widget-specific configuration values. You can find what values are available on each widget's page below.

You may use the same widget multiple times, configured in different ways.

``` .language-yaml
widgets:
  -
    type: collection
    collection: blog
    limit: 5
    width: half   # Can be 'half' or 'full'. Defaults to 'half'.
  -
    type: collection
    collection: things
    limit: 3
```

## Permissions {#permissions}

Each widget may be restricted depending on certain permissions.

``` .language-yaml
widgets:
  -
    type: collection
    collection: blog
    limit: 5
    permissions:
      - collections:blog:edit
```

## Available Widgets {#available-widgets}
