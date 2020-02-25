---
title: Toggle
overview: A tactile on/off switch. It's used to create boolean settings. True and false. Nice, simple, and uncomplicated.
image: /assets/fieldtypes/toggle.gif
id: ac5f8f98-616f-4621-a7ee-dbc8bbc15525
---
## Data Structure {#data-structure}

Flicking the toggle to the right sets to the value to `true`, left to `false`.

## Templating {#templating}

Toggle is used for boolean logic, so you can inlcude it in to your boolean logic strings:

When toggle is on the left (assuming our toggle fieldset name is 'switchy_thing')

``` .template
{{ if switchy_thing:true }} Do that thing {{ /if }}
```

Including the both ways

``` .template
{{  if switchy_thing:true }}
  Do that thing
  {{ elseif }} 
  Do the other thing
{{ /if }}
