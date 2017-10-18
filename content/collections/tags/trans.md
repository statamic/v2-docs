---
title: Translate
overview: Translates a string
id: c2a2d391-bbea-4082-95a4-41db2b8d0dc7
parameters:
  -
    name: key
    type: tagpart|string
    description: The key of the translation string to find. Include both the filename and string key delimited with dots. Can be used as a tag part or a `key` parameter.
  -
    name: any parameters
    type: string
    description: Any additional parameters will be treated as parameters that should be replaced in the string. 
  -
    name: count
    type: integer *1*
    description: When using `trans_choice`, this is the number that defines the pluralization.
---
This tag will retrieve a string from a language file in the current locale. It is the equivalent of the [trans and trans_choice methods provided by Laravel](https://laravel.com/docs/5.1/localization#basic-usage).

## Usage {#usage}

Get the `bar` string from the `site/lang/en/foo.php` translation file (where `en` is the current locale).

``` .language-php
<?php
return [
    'bar' => 'Bar!',
    'welcome' => 'Welcome, :name!',
    'apples' => 'There is one apple|There are many apples',
];
```

```
{{ trans:foo.bar }} or {{ trans key="foo.bar" }}
```

``` .language-output
Bar! or Bar!
```

### Parameters {#string-parameters}

Any additional tag parameters will be treated as parameters that should be replaced in the string. 

```
{{ trans:foo.welcome name="Bob" }}
```

``` .language-output
Welcome, Bob!
```

### Pluralization {#pluralization}

To pluralize, use the `trans_choice` tag with a `count` parameter.

```
{{ trans_choice:foo.apples count="2" }}
```

``` .language-output
There are many apples.
```
