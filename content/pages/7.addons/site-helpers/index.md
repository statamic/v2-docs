---
title: Site Helpers
id: 93eb331b-b092-44ff-bf7c-ebfc14924e9b
overview: >
  While addons can encompass a large feature set that might be shared, sold, or reused; site helpers provide a quick way to add functionality.
---

## Overview {#overview}

Site helpers can be thought of as one catch-all addon for your site.

If you have a little piece of functionality you need to add - like a quick filter or tag - throw the code into the helper file instead of crafting an entire addon.

## Files {#files}

A number of addon aspects (tags, filters, and modifiers) will have a single corresponding helper file. These are located in `site/helpers`.

``` .lang-files
site
`-- helpers
    |-- Tags.php
    |-- Modifiers.php
    `-- Filters.php
```

## Generating {#generating}

You may generate the helper files by running their corresponding commands:

```
php please make:tags-helper
php please make:modifiers-helper
php please make:filters-helper
```

If the files already exist, Statamic will prompt you for a method name which will be appended to the file.

## Tags {#tags}

Tags are used in your templates with the `site:` prefix. Much like how an addon's tag prefix would be the addon name.

``` .lang-statamic
{{ site:your_tag }}
```

This corresponds to the camelCased method:

```
<?php

namespace Statamic\SiteHelpers;

use Statamic\Extend\Tags as AbstractTags;

class Tags extends AbstractTags
{
    public function yourTag()
    {
        //
    }
}
```

Tags inside a site helper work almost identically to how [Tags](/addons/classes/tags) work within the context of an addon.

## Modifiers {#modifiers}

Modifiers are used in your templates just like bundled or addons' modifiers would. They correspond to the camelCased method name.

``` .lang-statamic
<!-- Pipe syntax -->
{{ variable | your_modifier:paramOne:paramTwo }}

<!-- Parameter syntax -->
{{ variable your_modifier="paramOne|paramTwo" }}
```

```
<?php

namespace Statamic\SiteHelpers;

class Modifiers
{
    public function yourModifier($value, $params, $context)
    {
        //
    }
}
```

## Filters {#filters}

Filters are used in your templates just like addons' filters.

``` .lang-statamic
{{ collection:blog filter="your_filter" }}
    ...
{{ /collection:blog }}
```

They correspond to the camelCased method name.

```
<?php

namespace Statamic\SiteHelpers;

use Statamic\Extend\Filter;

class Filters extends Filter
{
    public function yourFilter($collection)
    {
        return $collection->filter(function ($entry) {
            return $entry;
        });
    }
}
```

Filters inside a site helper work almost identically to how [Filters](/addons/classes/filters) work within the context of an addon.
