---
overview: >
  Controllers allow you to handle requests either through the front-end or the control panel.
  They let you organize request logic into a single class.
title: Controllers
id: 01b6596e-19c1-41e7-a3e1-e92144abc211
---
## Creating a Controller {#creating-a-controller}

An addon can have any number of controllers for usage through the Control Panel. For usage through the front-end of a site, you are limited to
a single controller. An addon may contain controllers for both Control Panel _and_ front-end usage.

To create a controller, you'll need to create a class file. You may either place your controllers in the root of your addon directory, or
nested within a `Controllers` namespace.

``` .lang-files
site/addons/Karma
|-- KarmaController.php 
|-- AnotherController.php
`-- Controllers
    `-- KarmaController.php
    `-- AnotherController.php
```

You can also use a command to generate the controller.

``` .lang-bash
php please make:controller AddonName
```

Here's a basic controller. Each public method in the class can receive a route.

``` .language-php
<?php

namespace Statamic\Addons\Karma;

use Statamic\Extend\Controller;

class KarmaController extends Controller
{
    public function getFoo()
    {
        return $this->view('foo', [
            'title' => 'Karma'
        ]);
    }
}
```

## Usage {#usage}

There are two ways routes can be directed into a controller. They differ depending on whether they are going through
the front-end of the site, or through the control panel.

### Front-end {#frontend}

A common use case for addons is to be able to have a form, which would need to be handled somehow. You could submit
that form to the method a controller would handle.

Whenever a request is made to `/!/foo/bar`, it will be routed into a corresponding controller method, prefixed with the
HTTP verb used. The method will be converted to camel case.

For example, assuming there's an addon named `Foo`:

- GET request to `/!/Foo/barBaz` or `/!/Foo/bar_baz` will call `FooController@getBarBaz`.
- POST request to `/!/Foo/barBaz` or `/!/Foo/bar_baz` will call `FooController@postBarBaz`, etc.

If you're familiar with Laravel, this can be thought of as similar to `Route::controller()`.

Any additional URL segments present get passed in as parameters. So for example: 

- GET request to `/!/Foo/display/people/123` will call `FooController@getDisplay` with arguments `people` and `123`.

If, for example, the method declares only one parameter, the `123` will simply be ignored.

### Control Panel {#control-panel}

We can route to the controller action above by editing the routes array in our addon's `routes.yaml` file.

``` .language-yaml
routes:
  foo: getFoo
```

The key is the route itself, and the value is the controller method

All of an addon's routes are prefixed by the addon route. So the `foo` route above would actually be `/cp/addons/karma/foo`.

By default, the controller used will be named after your addon.  
For example, `Statamic\Addons\Karma\KarmaController`.  

You may specify a controller by prefixing it with the class name.

``` .lang-yaml
routes:
  foo: FooController@index
```

This will look for either `Statamic\Addons\Karma\FooController` or `Statamic\Addons\Karma\Controllers\FooController`.


#### CP Routing schema {#cp-routing-schema}

Your routing array can handle more than basic GET requests.

``` .language-yaml
routes:

  # GET request to /cp/addons/addon-name using the index method
  /: index           

  # POST request to /cp/addons/addon-name/save using the save method
  post@save: save    

  # GET request to /cp/addons/addon-name/edit
  # - using the getEdit controller method
  # - named karma.edit, that you can reference by route('karma.edit')
  edit:              
    uses: getEdit    
    as: karma.edit
```

Note: If you want a settings page, you do _not_ need to create a route. A `/cp/addons/addon-name/settings` route
will be available to you automatically. To avoid conflicts you should not create a route named `settings`.
[See how to create a settings page](/addons/classes/settings).
