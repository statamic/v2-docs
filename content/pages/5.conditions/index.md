---
title: Conditions
id: 9751908a-a10c-4c36-abd3-2251e83fbc65
parse_content: false
overview: Conditions allow you filter listings by using simple English phrasing.
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

We can't forget about Scotch, but since its spelled `whiskey`, our condition filters it out.

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

## Available Conditions


### is|equals
type: mixed

Checks if a field is equal to a value. Appending `_strict` will use a `===` for comparison. eg. `foo:is_strict="bar"`

### not|isnt|aint
type: mixed

Checks if a field is not equal to a value. Appending `_strict` will use a `===` for comparison. eg. `foo:isnt_strict="bar"`

### exists|isset
type: bool

Checks if a field exists (or is not `null`)

### doesnt_exist|not_set|isnt_set|null
type: bool

Checks if a field doesn't exist (or is `null`)    

### contains
type: string

If the field is a string, it'll check that field contains a given string. If the field is an array, it'll check that the value is within the array.

### doesnt_contain
type: string

The opposite of `contains`.

### starts_with|begins_with
type: string

Checks if a field starts with a given string.

### doesnt_start_with|doesnt_begin_with
type: string

Checks if a field doesn't start with a given string.

### ends_with
type: string

Checks if a field ends with a given string.

### doesnt_end_with
type: string

Checks if a field doesn't end with a given string.

### matches|match|regex
type: regex

Checks if a field matches a given regular expression.