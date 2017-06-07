---
title: Fieldtypes
overview: >
  Fieldtypes are specialized form inputs designed to help manage content and data in the control panel. They are flexible and reusuable in all Content Types.
id: a6bd62fe-fa1e-4b34-aba8-c2d74b6d8dc7
---
## Overview {#overview}

Fieldtypes are [Vue.js][vue] components that have a corresponding PHP classes to facilitate any pre/post processing of data.

Having a good understanding of Vue.js will allow you to build more elaborate fieldtypes more readily. Their documentation is very helpful - it's worth a read. We've fallen in love with Vue.js and think you should too.

**Note**: We use Vue.js v1 in the Statamic Control Panel.


## Anatomy of a Fieldtype {#anatomy}

At its most basic level, a fieldtype requires an `AddonNameFieldtype.php` class and a corresponding `js/fieldtype.js` Vue component.

``` .language-files
site/addons/MyAddon/
|-- /resources/assets/js/
|   |-- fieldtype.js
|-- MyAddonFieldtype.php
```

## Multiple Fieldtypes {#multiple}

Since 2.6, an addon may have more than one fieldtype. One "primary" fieldtype, and multiple "secondary" fieldtypes.

### Directory Structure {#directory-structure}

You may store your modifier classes either in the root directory, like so:

``` .lang-files
site/addons/Bacon
|-- BaconFieldtype.php
|-- BitsFieldtype.php
`-- meta.yaml
```

...or within a `Fieldtypes` directory/namespace if you wish to stay more organized:

``` .lang-files
site/addons/Bacon
|-- Fieldtypes
|   |-- BaconFieldtype.php
|   `-- BitsFieldtype.php
`-- meta.yaml
```

You still only need one single `fieldtype.js` file. You may add multiple Vue components in it.

### Primary vs. Secondary {#primary-secondary}

An addon's primary fieldtype will use the name of the addon.  

``` .lang-yaml
field_name:
  type: your_addon
```

This will correspond to the `Statamic\Addons\YourAddon\YourAddonFieldtype` or `Statamic\Addons\YourAddon\Fieldtypes\YourAddonFieldtype` class.

Secondary fieldtypes will be use the name of the addon and the secondary name, delimited by a dot.

``` .lang-yaml
field_name:
  type: your_addon.secondary
```

This will correspond to the `Statamic\Addons\YourAddon\SecondaryFieldtype` or `Statamic\Addons\YourAddon\Fieldtypes\SecondaryFieldtype` class.


## Generating a Fieldtype {#generating}

The `php please make:addon` or `php please make:fieldtype` commands will create these for you - with some boilerplate code to get you going.

The code within the `fieldtype.js` file will be loaded automatically within the Control Panel.

**Note**: This will only generate a primary fieldtype in the root directory.

## The Javascript side {#javascript}

Vue.js lets us do work some magic with the power of two-way data-binding, like knowing and having access to the current state of all data in a Publish screen at all times.

Statamic calls the defined Vue.js component using the props `data`, `config` and `name`.

Let's dive into an example to make things clearer. Have you ever seen one of those password fields with a checkbox that toggles whether the text is visible? Let's build one of those.

<div class="screenshot padded" markdown=1>
  ![Secret Password? Nahhhh...](/assets/examples/password-fieldtype.gif)
</div>

Let's start with the Vue component.

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

    template: '' +
      '<div>' +
        '<input :type="inputType" v-model="data" />' +
        '<input type="checkbox" :id="name" v-model="show" />' +
        '<label :for="name">Show password</label>' +
      '</div>' +
    ''

});
```

### Props

The three `props` are passed into your fieldtype by a parent fieldtype builder component. You have to include that array just like you see above. The `data` prop contains the field's value, the `name` prop is the name of the field in the fieldset, and the `config` props is the config from the fieldset.

### Data 

`data` should be a function that returns an object containing any variables you want available in your component.

In the example, we're defining `show` to keep track of whether the value should be shown or not.

### Computed

The `computed` property is an object containing any values that should be dynamically evaluated.

In the example, we're letting `inputType` be the value for the input's `type` attribute - making it either a text or password field.

### Template

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

### Methods, Events, Ready...

Vue has more features you can use in your fieldtypes. Remember, a fieldtype is just a Vue component. You aren't limited to the items used in the example.

Refer to the [Vue component docs][vue-component] for more information.

## The PHP Side {#php}

The PHP side of your fieldtype is responsible for manipulating data before  the data is loaded or after it's submitted. This will let you enforce data types, set relationships, fire events, or any other number of things.

### Example Fieldtype Class {#example}

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

These are the default method values for a fieldtype. As you can see, we're just passing the data along untouched, but you could do anything you'd like here.

### Blank value {#blank-value}

The value returned by the `blank` method will be used as the "default" value for fields using your fieldtype. For example, maybe your field should be dealing with arrays by default. In that case, you should return `[]`.

### Processing {#processing}

The `preProcess` and `process` methods handle any manipulation of data coming to and from the field.

The `preProcess` method is called after data is read from file and before it's sent to the publish page. This could be useful if your Vue component needs the data in a different format than what's appropriate to be saved in files.

The `process` is the opposite: it's called after the publish page is submitted and before the data is written back to file.

In both cases, simply return the value after you've manipulated it.



[vue]: http://vuejs.org
[vue-component]: http://vuejs.org/guide/components.html
