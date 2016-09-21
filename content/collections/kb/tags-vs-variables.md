---
title: Tags vs. Variables
id: 616b0ff4-43a7-4652-a868-85b71f8f6176
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
---
In [Antlers templates][templates] there are two types of generic "tags". Simple variables, and Tags (a proper noun). They can both render things in your templates, so what's the difference, exactly?

## The TL;DR version {#tldr}

Variables simply render existing data. Tags can "do things".

## Understanding Antlers {#understanding-antlers}

The first step to really understanding the difference is to understand the basics of how Antlers works.

On a high level, we pass an array of data along with a template, and then parse it, replacing your tags with variables of the same name, if any are found. It might look something like this:

``` .language-php
$variables = [
    'favorite' => 'bacon',
    'brands' => [
        ['name' => 'Lagavulin', 'location' => 'Scotland'],
        ['name' => 'Jameson', 'location' => 'Ireland'],
  ]
];

$template = '
    My favorite food is {{ favorite }}.
    Here are some whisky brands:
    {{ brands }}
    {{ name }} from {{ location }}
    {{ /brands }}
    I am on the {{ env:APP_ENV }} environment.
';

return Parse::template($template, $variables);
```

Here you have a simple single tag variable (`favorite`) that just spits out the string value, and a tag pair (`brands`) that loops over the array and replaces child keys with their appropriate values.

The above would output something like this:

``` .language-output
My favorite food is bacon.
Here are some whiskey brands:
Lagavulin from Scotland
Jameson from Ireland
I am on the production environment.
```

But what happened with `{{ env:APP_ENV }}`? There's no env value anywhere in the array.

Antlers didn't find any `env` variable, so it looked outside the data to see if there is a Tag named `env`.
It just so happened that there was, so it executed the code in that Tag method, reading `APP_ENV` from your `.env` 
file, and returning the found value, which happened to be `production`.

### What variables get sent to your templates {#template-variables}

The above is an example of a hardcoded set of variables, but what _actually_ gets sent into your template?

- On all URLs, all the [global variables](/variables#global) are added for you.
- On all URLs, all of _your_ [globals](/content-types#globals) are added for you, too.
- If you're on an entry URL, that [entry's variables](/variables#entry) would be available. If you were on a Page, that [page's variables](/variables#page) would be available, and so on.

So, if only that set of data is sent to your template, how would you get data from other places?

That's where tags come in.

For instance, the [Collection Tag][collection_tag] will grab entries, and make their variables available to you within the tag pair.
Now, if you didn't notice, now the Collection Tag is basically doing what was explained above. The stuff between the tag pair is
another mini-template, and the [entry's variables](/variables#entry) is the data array. How meta!

## The differences {#differences}

### Usage in conditionals {#conditionals}

Tags require single braces inside conditions, variables do not.

Variable usage:

```
{{ if environment == "production" }}
```

Tag usage:

```
{{ if {env:APP_ENV} == "production" }}
```

### Modifiers and parameters {#modifiers-and-parameters}

Variables can have modifiers applied to them, tags cannot.

Tags can have parameters, variables cannot*.

Applying a modifier to a variable: OK!

```
food: bacon

{{ food | backspace:2 }}

bac
```

Applying a modifier to a tag: NO!

```
{{ some_tag | reverse }}
```

* However, the "parameter syntax" modifiers may look like Tag parameters.

Applying a modifier to a variable using param syntax:

```
{{ food backspace="2" }}

bac
```

Applying a modifier to a tag using param syntax:

```
{{ some_tag backspace="2" }}
```

This would only do something if the tag is expecting a parameter named backspace. It will not apply the backspace modifier.

[templates]: /antlers
[collection_tag]: /tags/collection