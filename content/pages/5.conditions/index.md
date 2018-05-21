---
title: Conditions
id: 9751908a-a10c-4c36-abd3-2251e83fbc65
parse_content: false
overview: Conditions allow you filter the results of your content tags (e.g. Collections, Taxonomies) with the data inside them, much like WHERE clauses do with SQL.
details:
  -
    name: is|equals
    type: mixed
    description: Checks if a field is equal to a value. Appending `_strict` will use a `===` for comparison. eg. `foo:is_strict="bar"`
  -
    name: not|isnt|aint
    type: mixed
    description: Checks if a field is not equal to a value. Appending `_strict` will use a `===` for comparison. eg. `foo:isnt_strict="bar"`
  -
    name: exists|isset
    type: bool
    description: Checks if a field exists (or is not `null`)
  -
    name: doesnt_exist|not_set|isnt_set|null
    type: bool
    description: Checks if a field doesn't exist (or is `null`)    
  -
    name: contains
    type: string|array
    description: Check if a field contains a given string or if a value is within an array.
  -
    name: doesnt_contain
    type: string|array
    description: The inverse of `contains`.
  -
    name: starts_with|begins_with
    type: string
    description: Checks if a field starts with a given string.
  -
    name: doesnt_start_with|doesnt_begin_with
    type: string
    description: Checks if a field doesn't start with a given string.
  -
    name: ends_with
    type: string
    description: Checks if a field ends with a given string.
  -
    name: doesnt_end_with
    type: string
    description: Checks if a field doesn't end with a given string.
  -
    name: greater_than|gt
    type: mixed
    description: Checks if a field is greater than a value.
  -
    name: less_than|lt
    type: mixed
    description: Checks if a field is less than a value.
  -
    name: greater_than_or_equal_to|gte
    type: mixed
    description: Checks if a field is greater than or equal to a value.
  -
    name: less_than_or_equal_to|lte
    type: mixed
    description: Checks if a field is less than or equal to a value.
  -
    name: matches|match|regex
    type: regex
    description: Checks if a field matches a given regular expression.
  -
    name: boolean modifiers
    type: string
    description: |
      Any [modifiers](/docs/modifiers) that return booleans may be used (eg. `is_past`, `is_future`, etc) [More details][link]
      [link]: #boolean-modifiers
---
## Usage {#usage}

The syntax is for all the available conditions is `field:condition="value"`.

For conditions where you may specify the comparison value, like `is` or `starts_with`, you'd do:

```
my_field:starts_with="something"
```

For boolean conditions, like `exists` or `null`, you should simply specify a value of `true`, to trigger the condition:

```
my_field:exists="true"
```

For negative boolean conditions, _don't_ use `="false"`. Instead, pick the opposite condition.  
For example, `:exists` vs. `:doesnt_exists`.

```
<!-- Don't do this: -->
my_field:exists="false"

<!-- Do this: -->
my_field:doesnt_exist="true"
```


## Example {#example}

Let's say we have a collection of beverages:

``` language-yaml
- Amaretto
- Bourbon Whiskey
- Gin
- Irish Whiskey
- Scotch Whisky
- Rye Whiskey
- Rum
- Vodka
```

But we only want to show whiskeys, because they are delicious.

```
{{ collection:drinks title:contains="whiskey" }}
    {{ title }}
{{ /collection:drinks }}
```

That would filter down to this:

``` language-yaml
- Bourbon Whiskey
- Irish Whiskey
- Rye Whiskey
```

We can't forget about Scotch, but since its spelled `whisky`, our condition filters it out.

Let's use a regular expression to make that `e` optional.

```
{{ collection:drinks title:match="/whiske?y/" }}
    {{ title }}
{{ /collection:drinks }}
```

Now Scotch will be in the list, and we can sleep soundly knowing that no whiskey was forgotten.

If you want narrow your results further you can repeat conditions as many times as needed.

```
{{ collection:drinks title:is="whiskey" country:is="scotland" }}
    {{ title }}
{{ /collection:drinks }}
```

## Boolean Modifiers {#boolean-modifiers}

In addition to the list of conditions below, you are free to use any [modifiers](/docs/modifiers) that return booleans (for example, `is_past` or `is_future`).

Similar to the native boolean conditions, you can use these modifiers as conditions by passing in a value of `true`.

```
{{ collection:events event_date:is_future="true" }}
```


## Conditions List {#conditions}
