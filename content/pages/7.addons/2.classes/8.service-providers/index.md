---
title: Service Providers
overview: >
  Statamic is built on top of Laravel, which makes heavy use of Service Providers to bootstrap functionality into the application. Your own addons, as well as all of Statamic's core services are bootstrapped via service providers.
id: 35d4de1b-a602-4b52-9608-0566c0d65e2b
---
## Overview {#overview}

Service providers are the central place of all Statamic, application bootstrapping. Your own addons, as well as all of Statamic's core services are bootstrapped via service providers.

But, what do we mean by "bootstrapped"? In general, we mean registering things, including registering service container bindings, event listeners, middleware, and even routes.

## Example Class {#example-class}

The class file must be named `AddonNameServiceProvider.php`.

``` .language-php
<?php

namespace ExampleNamespace;

use Illuminate\Support\ServiceProvider;

class ExampleClass extends ServiceProvider
{
    /**
     * Bootstrap the application services.
     *
     * @return void
     */
    public function boot()
    {
        //
    }

    /**
     * Register the application services.
     *
     * @return void
     */
    public function register()
    {
        //
    }
}
```

## Generating {#generating}

You can generate a service provider class file with a console command.

``` .language-console
php please make:provider AddonName
```

## Learn More {#learn-more}

[Read more about Service Providers][providers] in the Laravel docs.

[providers]: http://laravel.com/docs/5.1/providers
