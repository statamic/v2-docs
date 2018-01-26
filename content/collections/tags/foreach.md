---
title: Foreach
description: Iterate over an array with key/value pairs.
overview: Iterate over an array with key/value pairs.
parameters:
  - 
    name: as
    type: 'string'
    description: Defines the key/value variable names. Defaults to `key|value`.
variables:
  - 
    name: key
    type: string
    description: The key for each item in the array. The variable name can be changed from `key` to something else with the `as` parameter.
  - 
    name: value
    type: mixed
    description: The value for each item in the array. The variable name can be changed from `value` to something else with the `as` parameter.
id: 4e5d8156-433a-48ce-985c-456634527a37
---
This tag is designed to loop over the key/value pairs in a simple named list.

For example, you have a list of cooking temperatures, which you've defined on the fly with the [Array Fieldtype](/fieldtypes/array).

``` .language-yaml
cooking_temps:
  Chicken: 165
  Beef: 145
```

You can loop over them like this:

```
{{ foreach:cooking_temps }}
  {{ key }} should be cooked at {{ value }}°F
{{ /foreach:cooking_temps }}
```

You may also use the `as` parameter to define the names of the variables. This may help make your templates more understandable.

```
{{ foreach:cooking_temps as="food|temperature" }}
  {{ food }} should be cooked at {{ temperature }}°F
{{ /foreach:cooking_temps }}
```

Or, you may just specify the value's variable name. The key will remain as `key`.

```
{{ foreach:cooking_temps as="temperature" }}
  {{ key }} should be cooked at {{ temperature }}°F
{{ /foreach:cooking_temps }}
```

All of the above examples would render:

``` .language-output
Chicken should be cooked at 165°F
Beef should be cooked at 145°F
```
