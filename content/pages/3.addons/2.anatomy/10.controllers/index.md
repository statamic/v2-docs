---
overview: >
  Controllers let you create pages in the
  control panel. They let you organize
  request logic into a single class.
title: Controllers
id: 01b6596e-19c1-41e7-a3e1-e92144abc211
---
## Creating a Controller

To create a controller, you will need a class file named `[AddonName]Controller.php`. eg. `KarmaController.php`.

You can also use `php please make:controller AddonName` to have one generated for you.

Here's a basic controller:

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

Each public method in the controller can have a route pointed toward it. We can route to the controller action above by editing the routes array in our addon's `meta.yaml` file.

``` .language-yaml
routes:
  foo: getFoo
```

The key is the route itself, and the value is the controller method.

All of an addon's routes are prefixed by the addon route. So the `foo` route above would actually be `/cp/addons/karma/foo`.


## Routing schema

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
