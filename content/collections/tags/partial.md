---
title: Partial
overview: Render a partial template.
parameters:
  -
    name: partial
    type: tag part
    description: "The name of the partial file, relative to the `partials` folder. This is part of the tag, not actually a parameter. For example, you'd use `partial:list` to load the `list` partial."
  -
    name: variables
    type: multiple params
    description: >
      Any additional parameters specified will
      be sent into the partial as variables.
      To pass in a variable, just use its
      name. More information below.
id: 1f683992-401e-44f6-8506-7967005778a5
---
## Example 1: The header {#example-header}

The most common usage of a partial is to add a header or navigation into a layout file, like this:

```
<html>
<body>
  {{ partial:header }}

  <div class="main">
    {{ template_content }}
  </div>
</body>
</html>
```

The `{{ partial:header }}` tag would output the contents of `site/themes/theme_name/partials/header.html`. Note no space is allowed between the tag’s trailing colon your partial’s filename. I.e., `{{ partial:header }}` works, whereas `{{ partial: header }}` does not.

Splitting your templates up into partials is a nice way to keep things clean and organized.

## Example 2: The reusable chunk {#example-reuse}

Another common usage for a partial is to pass in data into a reusable chunk of template code.

Here's a partial which outputs a list.

```
<h3>{{ header }}</h3>
<ul>
  {{ items }}
    <li>{{ value }}</li>
  {{ /items }}
</ul>
```

In a page, we might have a list of animals.

``` language-yaml
animals:
  - Stag
  - Rhino
  - Alligator
```

Then in our template, we want to output those animals using the partial.

```
<div>
  {{ partial:list header="Favorite Animals" items="animals" }}
</div>
```

Any parameters that are passed into the `partial` tag will be used as variables within the partial. So here we have `header` and `items`.

Statamic will look for variables first, and then just treat them as strings.

In the example, there is no variable named `{{ Favorite Animals }}`, so it will just use `Favorite Animals`, the string.

But there _is_ an `{{ animals }}` variable, so it'll pass that through to the partial as `{{ items }}`.

The resulting output would look like this:

```
<h3>Favorite Animals</h3>
<ul>
  <li>Stag</li>
  <li>Rhino</li>
  <li>Alligator</li>
</ul>
```
