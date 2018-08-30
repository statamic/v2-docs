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

Secondary fieldtypes will use the name of the addon and the secondary name, delimited by a dot.

``` .lang-yaml
field_name:
  type: your_addon.my_secondary
```

This will correspond to the `Statamic\Addons\YourAddon\MySecondaryFieldtype` or `Statamic\Addons\YourAddon\Fieldtypes\MySecondaryFieldtype` class.

> **Note**: In your `fieldtype.js` file, secondary components follow this naming convention: `your_addon-my_secondary-fieldtype`.


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

    mixins: [Fieldtype],

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

### Fieldtype Mixin

This mixin provides some basics for you automatically.  

It gives you the three required `props` that are passed into your fieldtype by a parent fieldtype builder component. The `data` prop contains the field's value, the `name` prop is the name of the field in the fieldset, and the `config` props is the config from the fieldset.

It also sets things up like data value change watchers. [See below for more information](#data-change-watcher).

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

### Data Change Watcher {#data-change-watcher}

Have you ever filled out a long form and accidentally navigated away from the page, only to lose all your progress?
That's really annoying! The publish page component will monitor fieldtypes for any changes, and prevent users from navigating away by mistake.

For simple fieldtypes, this is done automatically just by making sure to mix in the `Fieldtype` mixin, as demonstrated above.
This mixin will fire up an event watcher, monitoring your `data` prop for any changes and telling the publish component that something's different.

If your fieldtype is more complex, there are a couple of steps for you to add to get this going. By "more complex" we mean you might perform
some initial data modification once your component is ready, like AJAX loading or setting a default value. 

First, tell the fieldtype to _not_ automatically bind the watcher. Then, bind it manually when you're ready. Take this example that needs to adjust the initial data after an AJAX request.

``` .language-js
{
    mixins: [Fieldtype],

    data: function () {
        return {
            autoBindChangeWatcher: false // Disable the automagic binding
        }
    },

    ready: function () {
        this.$http.get('/example', function (response) {
            this.data = response.data.something; // Manipulate as required.
            this.bindChangeWatcher(); // Bind manually once you're ready.
        });
    }
}
```

### Focus {#focus}

When Statamic wants to programatically focus your field (for example, when adding a new Replicator set or Grid row), it needs to know how to do so.

By default, Statamic will simply call `.focus()` on your Vue component's `$el` property (a DOM element). This will work fine if you have a simple fieldtype like the `text` field.

You may customize how focusing works by adding a `focus()` method to your Vue component. For example:

``` .language-javascript
template: `<div><div><input type="text" v-el:my-custom-element /></div></div>`,
methods: {
    focus() {
        this.$els.myCustomElement.focus();
    }
}
```

### Replicator Preview Text {#replicator-preview-text}

When [Replicator](/fieldtypes/replicator) sets are collapsed, Statamic will display a preview of the data within it.

By default, Statamic will do its best to display your fields data. However, if you have a value more complex than a simple string or array, you may want to customize it.

You may customize the preview text by adding a `getReplicatorPreviewText()` method to your Vue component. For example:

``` .language-javascript
methods: {
    getReplicatorPreviewText() {
        return this.data.join('+');
    }
}
```


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


## Extras

### Auto-slugify {#auto-slugify}

If you have a need to automatically create a slug in one place based off the value of another field, we have a Vue mixin you can use in your component.
An example of this is when you type into a `title` field and the `slug` field gets generated as you type.

``` .language-js
{
    mixins: [Fieldtype, AutoSlug],

    ready() {
        this.autoSlug()
    }
}
```

The `autoSlug` method takes 2 arguments:

- The name of the field to generate a slug from. In the example above, this would be `"title"`.
- The name of an instance property on your Vue component where the slug should be stored. If left blank, it will be `"data"` which is like doing `this.data = newSlug`.

> If you're thinking about using this to make a simple slugging field, the [Text fieldtype](/fieldtypes/text) has an `autoslug` option that does this.



[vue]: http://vuejs.org
[vue-component]: https://v1.vuejs.org/guide/components.html
