---
title: Fieldtypes
overview: >
  Fieldtypes create new ways for you to
  enter content into the control panel.
id: a6bd62fe-fa1e-4b34-aba8-c2d74b6d8dc7
---
## What's in a fieldtype?

A fieldtype in Statamic v2 is primarily made up of Javascript. Specifically, [Vue.js][vue].  
All fieldtypes are Vue components with a little PHP to help out behind the scenes with things like data processing.

Having a good understanding of Vue.js will allow you to build more complex fieldtypes more easily. Their documentation is very helpful - it's worth a read.


## Creating a fieldtype

The bare minimum fieldtype requires you to have an `AddonNameFieldtype.php` file, and a `js/fieldtype.js` file.


``` .language-files
site/addons/MyAddon/
|-- js/
|   |-- fieldtype.js
|-- MyAddonFieldtype.php
```

The `please make:addon` or `please make:fieldtype` commands will create these for you - with some boilerplate code to get you going.

The code within the `fieldtype.js` file will be loaded automatically within the Control Panel.

## The Javascript side

Vue.js lets us do some great things which wouldn't be possible without the powerful included two-way data-binding features.

Everything is more easily understood with an example, so let's dive into one.

Everyone's seen one of those password fields with a checkbox that toggles whether the text is visible.

![](/assets/examples/password-fieldtype.gif)

We'll be creating one of those. 

``` .language-javascript
Vue.component('password_toggle-fieldtype', {

    props: ['data', 'config', 'name'],

    data: function() {
        return {
            show: false
        };
    },

    computed: {
        inputType: function() {
            return this.show ? 'text' : 'password';
        }
    }, 

    template: '
      <div>
        <input :type="inputType" v-model="data" />
        <input type="checkbox" :id="name" v-model="show" />
        <label :for="name">Show password</label>
      </div>
    '

});
```

### So, what's going on here?

#### Props

The three `props` are passed into your fieldtype behind the scenes. You have to include that array just like you see above. The `data` prop contains the field's value, the `name` prop is the name of the field in the fieldset, and the `config` props is the config from the fieldset.

#### Data 

`data` should be a function that returns an object containing any variables you want available in your component.

In the example, we're defining `show` to keep track of whether the value should be shown or not.

#### Computed

The `computed` property is an object containing any values that should be dynamically evaluated.

In the example, we're letting `inputType` be the value for the input's `type` attribute - making it either a text or password field.

#### Template

`template` is the html that'll be rendered where you'd expect to see your fieldtype.

You should try to make sure that everything is contained within one root element - like the `div` in the example.

Let's take a closer look at the template:

```
<input :type="inputType" v-model="data" />
<input type="checkbox" :id="name" v-model="show" />
<label :for="name">Show password</label>
```

The colons and `v-`'s you see is Vue's binding syntax at work.

The colons will evaluate the value and turn it into the attribute. So, `:type="inputType"` will either be `type="text"` or `type="password"`, depending on the value of `inputType`.

The `v-model` attributes will bind the values to the form input. If you type into the text box, `data` will get updated. If you check the checkbox, `show` will get updated.

#### Methods, events, ready...

Vue has more features you can use in your fieldtypes. Remember, a fieldtype is just a Vue component. You aren't limited to the items used in the example.

Refer to the [Vue component docs][vue-component] for more information.

## The PHP side

The PHP side of your fieldtype is responsible for manipulating values.

Take this boilerplate code:

``` .language-php
<?php

namespace Statamic\Addons\MyAddon;

use Statamic\Extend\Fieldtype;

class MyAddonFieldtype extends Fieldtype
{
    public function blank()
    {
        return null;
    }

    public function preProcess($data)
    {
        return $data;
    }

    public function process($data)
    {
        return $data;
    }
}
```

These are the default method values for a fieldtype. As it stands, they don't do anything and could really be removed. However, if you want to modify the default functionality, you can edit these.

#### Blank value

The value returned by the `blank` method will be used as the "default" value for fields using your fieldtype. For example, maybe your field should be dealing with arrays by default. In that case, you should return `[]`.

#### Processing

The `preProcess` and `process` methods handle any manipulation of data coming to and from the field.

The `preProcess` method is called after data is read from file and before it's sent to the publish page. This could be useful if your Vue component needs the data in a different format than what's appropriate to be saved in files.

The `process` is the opposite: it's called after the publish page is submitted and before the data is written back to file.

In both cases, simply return the value after you've manipulated it.



[vue]: http://vuejs.org
[vue-component]: http://vuejs.org/guide/components.html
