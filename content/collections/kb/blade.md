---
title: Using Laravel Blade for templating
id: efa34b0d-b4c3-43fc-bb91-a6d331dc6026
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
---
## The Blade Templating Language

You can find out how to use Blade, the templating language over on the [Laravel docs](https://laravel.com/docs/5.2/blade).
Here we'll be explaining how to use it in the context of Statamic.

## Using in a theme

Firstly, you'll want to check out how to create a theme in our [Theming Guide](/theming).

To use Blade in a theme, you'll need to add `.blade.php` templates instead of `.html` templates. You can mix and
match between Antlers and Blade template files, just be careful not to overlap the template names.

For instance, telling a page to use the `foo` template:

``` .language-yaml
template: foo
```

``` .language-files
your-theme/
`-- templates/
    |-- foo.html
    `-- foo.blade.html
```

There are two `foo` templates. Which one should be used? Don't make this mistake.

### Extends and Includes

When using `@extends` or `@include`, be aware that the files will be located in your `templates` directory.


## Accessing Data

When using a Blade template, you will get access to the top level `page` data. For example, if you were to
visit `/blog/my-first-post`, which is an entry with the following data:

``` .language-yaml
---
title: My First Post
favorite_things:
  - bacon
  - whisky
gamertags:
  swanson45: Ron Swanson
  npeppers: Niles Peppertrout
---
This is the first post, how exciting.
```

Your Blade template would have access to the variables in this file:

``` .language-blade
@extends('site')

<h1>{{ $title }}</h1>

@foreach ($favorite_things as $thing)
    {{ $thing }}
@endforeach

@foreach ($gamertags as $tag => $name)
    {{ $name }} goes by {{ $tag }}
@endforeach

{{ $content }}
```

## Modifiers

You can use [Modifiers](/modifiers) in your Blade templates, but they will use a different syntax than what you're used to with Antlers.

First, wrap your variable in a `modify()` method, then feel free to chain modifiers as you wish. The value will get passed along like normal. Any parameters should be specified like regular PHP parameters.

``` .language-blade
{{ modify($content)->striptags()->backspace(1)->ensureRight('!!!') }}
```

``` .language-output
THIS IS THE FIRST POST, HOW EXCITING!!!
```

Note that when using multi-word modifiers, like `ensure_right`, you can either use the documented snake_case version, or use the camelCased version.
