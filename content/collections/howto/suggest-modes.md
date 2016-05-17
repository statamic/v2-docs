---
title: How to use Suggest Modes in your addon
overview: >
  The Suggest field allows you to define a list of suggestions. You can get those from wherever you want: your own list, somewhere on the internet, or your local grocer.
id: dd9212d0-978c-4b86-a51f-745464ff0721
---
The "[Relate](/reference/fieldtypes/relate)" fields – "[Users](/reference/fieldtypes/users)", "[Taxonomy](/reference/fieldtypes/taxonomy)", "[Collection](/reference/fieldtypes/collection)" – all leverage the "[Suggest](/reference/fieldtypes/suggest)" field behind the scenes.

While the Relate fields might not look like a Suggest field, they follow the same basic principle: they retrieve "suggestions" through an AJAX call.

Here's the workflow - we'll use the "Users" fieldtype as an example:

1. The Users fieldtype Vue component[^1] is loaded. It's basically a wrapper around the Relate fieldtype, but passes a `type` of `user`.
2. The Relate fieldtype Vue component[^2] is loaded.
3. The Relate fieldtype performs an AJAX call to `/cp/addons/suggest/suggestions`, POSTing the field config.
4. That URL corresponds to the Suggest bundle's Controller[^3] where it calls `->suggetions()` on the appropriate `Mode` class.
5. In this case, the `UsersMode` class[^4] is loaded, and gathers the suggestions.

## Creating your own Suggest Modes

In step 4 of the workflow above, if you dig into the code, you will notice that it first checks for a Mode class in the Suggest addon, but then checks for a `Statamic\Addons\{$mode}\{$mode}SuggestMode` class. That's the secret sauce right there.

To use a custom mode, you can specify the mode in the Suggest field's config.

We'll create a mode that gets types of Bacon. This is Statamic after all.

``` .language-yaml
fields:
  things:
    type: suggest
    mode: Bacon
```

This'll attempt to load the `Statamic\Addons\Bacon\BaconSuggestMode` class, so let's create that at `site/addons/Bacon/BaconSuggestMode.php`.

``` .language-php
<?php namespace Statamic\Addons\Bacon;

use Statamic\Addons\Suggest\Modes\AbstractMode;

class BaconSuggestMode extends AbstractMode
{
    public function suggestions()
    {
        return [
            ['value' => 'applewood', 'text' => 'Applewood'],
            ['value' => 'hickory', 'text' => 'Hickory']
        ];
    }
}
```

The class should extend the `Statamic\Addons\Suggest\Modes\AbstractMode` class, and needs to have a single `suggestions()` method.

The `suggestions` method needs to return a multidimensional array. As you can see, each suggestion should have a `value` which will be saved, and the `text` that will be displayed. You can create this array however you want. Here we've just hardcoded it, but you could perform third party API calls, retrieve content, whatever!

## I don't want it to look like the Suggest field

Well, that's a different story. It'll be up to you implement the JS/Vue component.

As long as you perform the AJAX call to `/cp/addons/suggest/suggestions`, and POST your `mode` there, you'll be able to use the suggestion class methods as mentioned earlier.

You can check out the Relate fieldtype Vue component[^2] for inspiration. That's a completely customized fieldtype that leverages the Suggest modes behind the scenes.



[^1]: Located at `statamic/resources/js/components/fieldtypes/users/users.js`
[^2]: Located at `statamic/resources/js/components/fieldtypes/relate/relate.js`
[^3]: Located at `statamic/bundles/Suggest/SuggetsController.php@suggestions`
[^4]: Located at `statamic/bundles/Suggest/Modes/UsersMode.php`
