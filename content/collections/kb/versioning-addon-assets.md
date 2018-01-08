---
title: Versioning Addon Assets
kb_categories:
  - Tips, Tricks, and How-Tos
id: e6f184c1-0309-46eb-a156-f462bb0a6321
---

Addons may have assets (stylesheets, javascript files, etc) which can be loaded automatically or programatically depending on the situation.

Statamic will automatically load any addon's `resources/assets/js/fieldtype.js` and `scripts.js` files. You may also reference files in your addon by doing `$this->js->url('foo')` to output a URL to `resources/assets/js/foo.js`.

However, if you want to version (or cache bust) your assets, this may not work for you. To remedy this, Statamic allows you to customize the file paths that are used in URL generation by binding an `AddonName.resolve-resource-path` callback function into the container.

``` .language-php
public function register()
{
    $this->app->bind('YourAddonName.resolve-resource-path', function () {
        /**
         * @param  string  $path   The filename relative to the `resources/assets` directory. 
         *                         eg. `js/foo.js`
         * @param  Addon   $addon  An instance of Statamic\Extend\Addon for your addon.
         * @return string          The filename relative to `resources/assets`
         *                         eg. `js/foo.js?id=1234` or `js/foo.somehash.js`
         */
        return function ($path, $addon) {
            return $path;
        });
    });
}
```

## Laravel Mix Example

In this example, we version the fieldtype js file using mix's `version` method. An `id` parameter gets appended to the path inside `mix-manifest.json`. We then use that inside the callback.


``` .language-js
// webpack.mix.js
let mix = require('laravel-mix');
mix.setPublicPath('./');

// Source lives in `src/js/fieldtype.js` and gets compiled to `js/fieldtype.js`
mix.js('resources/assets/src/js/fieldtype.js', 'resources/assets/js');

// The manifest gets updated with a new id.
mix.version();
```

``` .language-files
/resources/assets/
|-- js/
|   `-- fieldtype.js
`-- src/
    `-- js/
        `-- fieldtype.js
```


``` .language-js
// mix-manifest.json
{
    "/resources/assets/js/fieldtype.js": "/resources/assets/js/fieldtype.js?id=123"
}
```

``` .language-php
$this->js->url('fieldtype'); // returns `js/fieldtype.js?id=123` as per the manifest
``` 

## Custom Webpack Example
In this example, the addon's build process will save the new version of the asset with a string within the filename. eg. `foo.js` becomes `foo.somehash.js`

``` .language-files
resources/assets/js/
|-- bar.js
`-- foo.123.js
```

``` .language-php
$this->js->url('foo'); // returns js/foo.123.js because it matched the regex
$this->js->url('bar'); // returns js/bar.js because there were no matching files
``` 

``` .language-php
<?php

namespace Statamic\Addons\MyAddon;

use Statamic\API\Str;
use Statamic\API\URL;
use Statamic\API\Path;
use Statamic\API\Folder;
use Statamic\Extend\ServiceProvider;

class MyAddonServiceProvider extends ServiceProvider
{
    public function register()
    {
        $this->app->bind('MyAddon.resolve-resource-path', function () {
            return function ($path, $addon) {
                // Make the regex pattern for matching the files.
                // Adds a "dot asterisk" before the extension in the basename (no directory)
                // eg. js/foo.js becomes /foo\..*\.js$/
                $pattern = '/' . preg_replace('/\.(\w+)$/', '\..*\.$1', pathinfo($path)['basename']) . '$/';

                // Get the path to the asset resources (relative to the project root)
                $assetsPath = Path::makeRelative($addon->directory() . '/resources/assets') . '/';

                // The directory to look for assets. We may have been provided `js/foo.js` so
                // we're going too look for files within the resources/assets/js directory.
                $directory = dirname($assetsPath . $path);

                $files = collect_files(Folder::getFiles($directory))->filterByRegex($pattern);

                // If no matching files were found, we'll fall back to the original 
                // (without any versioning).
                $path = $files->isEmpty() ? $path : $files->first();

                // Statamic is expecting a path relative to the addon's 
                // resources/assets directory.
                return Str::removeLeft($path, $assetsPath);
            };
        });
    }
}
```