---
title: Antlers Templates
id: 88472f52-93db-413c-a266-65014390c3f6
overview: >
  Statamic has its own template language designed to make complex things simple. It does most of the hard work when it comes to fetching, formatting, filtering, and displaying your content. If you know even just a little HTML, you’ll understand the basics rather quickly.
---

## Overview

The most basic aspect of the Antlers template language is the tag (lowercase “t.”, an important distinction which we'll make later). It can be and do many things, <span class="highlight">just like you -- if you don't give up and ignore the people discouraging you.</span>

```
This is a tag: {{ tag }}
```

There you have it. A pair of double curly braces with some stuff in the middle. Like an Oreo. Some people call these **squiggly braces**, **double mustaches**, **curly brackets**, and we’ve even heard them called **squirrelly brackets**. We like calling them **antler braces**. Because they look like antlers.

Nearly all of the power of Statamic is at your disposal in one way or another in the form of these tags. They're used in any Statamic template, layout, or partial - essentially any file in your theme ending in `.html`.

## Tag Syntax {#tag-syntax}

It’s important to cover how these tags are designed to be used, note a few of their important attributes, and then cover how they’re designed _not_ to be used.

### Braces stick together {#braces-stick-together}

Each set of double curly braces **must** always, at all times, stick together. Think of them as friends, or lovers if you must. Feel free to give them names like Turner and Hooch or Troy and Abed, but the left two and the right two are each as one.

All of the examples and styles are perfectly fine, technically. Use whatever style you like best, just be consistent with it or your templates will probably look messy, and then one day you will be super annoyed with yourself.

```
{{ tag }}         This one is best
{{tag}}           This is okay
{{ tag}}          Stop it
{{tag       }}    OMG WHY
```

### Tags are CaSe SeNsiTivE {#case-sensitive}

This means that `{{ title }}` and `{{ Title }}` are **two separate tags** that won’t behave the same. Additionally, hyphens and underscores are not interchangeable. The difference can be subtle. Don't let it bite you.

### Be Consistent! {#be-consistent}

Most important of all, be consistent. We recommend keeping all of your variable names lowercase and using underscores. It’s a simple and effective convention. We'll be using it everywhere in the docs.

## Variables {#variables}

The most basic tag type is the variable. A `{{ random }}` tag in a basic template will try to find the `random` key in your current context and display its value. If there is no `random` key, the parent contexts will be checked recursively. If the top context is reached and the key is still not found, Statamic will check to see if it’s a [Tag](#tags). If no Tag is found, the variable is set to `null` and nothing is rendered.

Variables can be strings or arrays/lists of data. Rendering these array variables is accomplished using a tag pair. Tag pairs are a set of tags that work together in unison: An opening tag and a closing tag. Much like HTML tags.

As there are several types of arrays and YAML list formats, there are also several ways to render the variables between the tag pairs. Let's cover all of the above.

### Single Variables {#single-variables}

Time for an example. Let’s start with some basic data. If you’re not familiar with YAML or haven’t read through the [YAML 101][guide-yaml] guide yet, it might be a good time to brush up.

```language-yaml
location: The Sahara Desert
temperature: 110
sincerity_level: Love
```

```
Hi {{ recipient }}! Today we are in {{ location }}.
It’s roughly {{ temperature }} degrees.
We are having a good time.

{{ sincerity_level }},
Benedict
```

```language-output
Hi ! Today we are in The Sahara Desert.
It’s roughly 110 degrees.
We are having a good time.

Love,
Benedict
```

If you’ve been following along you may have noticed Benedict’s classic blunder. He forgot to set a name for his recipient. Later we’ll suggest some ways to make his template a little smarter and establish some fallbacks, but for now? Mission accomplished with only one minor issue, an extraneous space.


### Multidimensional Arrays {#multidimensional-arrays}

A multi-dimensional array is an array that contains one or more arrays. Whether one or one hundred nested so deep your eyes cross as you dream about dreams inside dreams, they are each rendered with the same general approach.

```language-yaml
officers:
  - name: Charles Boyle
    attribute: a detective
  - name: Terry Jeffords
    attribute: a Sergeant
  - name: Jake Peralta
    attribute: smort
```

```
{{ officers }}
  {{ name }} is {{ attribute }}.
{{ /officers }}
```

```language-output
Charles Boyle is a detective.
Terry Jeffords is a Sergeant.
Jake Peralta is smort.
```

You can access arrays inside your arrays with tag pairs inside your tag pairs. And that’s all there is to it.

## Lists {#lists}

Lists are nothing more than simple arrays. They're also known as single-dimensional arrays, linear arrays, numerical arrays, YAML lists, or just arrays. Those are things you can Google if you'd like.

```language-yaml
national_parks:
  - Acadia
  - Denali
  - Redwood
```

As you may have guessed, you can access the variable with `{{ national_parks }}`. Since this is an array though, that's only the beginning. Add a closing tag, `{{ /national_parks }}`, and some helper variables you can use to display the bits inside, and you're in action. You can fetch the key (the order, starting from zero) with `{{ key }}` and the value (the reason we’re here) with `{{ value }}`.

```
{{ national_parks }}
  {{ value }}!
{{ /national_parks }}
```

```language-output
Acadia!
Denali!
Redwood!
```

## Variable Modifiers {#variable-modifiers}

Now comes the really fun part. Let's imagine you have a kit of carrier pigeons in your roost. Yes, that's what a group of pigeons is called. They've been patiently cooing and shedding feathers that you plan to harvest later to make a new pillow. They're waiting for a mission, and your Carrier Pigeon as a Service website just needs to show two pigeons, not all of them. You're in luck. Modifiers to save the day.

```language-yaml
pigeons:
  -
    name: Chester
    breed: Oriental Roller
    missions: 8
  -
    name: Longbottom
    breed: Showpen Homer
    missions: 3
  -
    name: Alister
    breed: Arad Tumbler
    missions: 12
```

```
{{ pigeons limit="2" }}
  {{ name }} is an experienced {{ breed }}.
{{ /pigeons }}
```

```language-output
Chester is an experienced Oriental Roller.
Longbottom is an experienced Showpen Homer.
```

### Chaining Modifiers {#chaining-modifiers}

You can string as many modifiers as you'd like in a row and each will be executed in sequence, with the altered data passed right along to the next modifier, like a donut frosting assembly line. This allows you to sort data before filtering it, check if some condition exists or a date is in the future before continuing, you name it.

With over [130 modifiers][modifiers] the number of possibilities are too hard to calcuate without Googling how first. It probably has something to do with factorials.

Let's elaborate on the pigeon example and sort the list by experience before limiting it.

```
{{ pigeons sort="missions:desc" limit="2" }}
  {{ name }} has {{ missions }} successful carries.
{{ /pigeons }}
```

```language-output
Alister has 12 successful carries.
Chester has 8 successful carries.
```

### Syntax Options {#syntax-options}

Modifiers can be written two different ways to help with readability and to simplify different use cases.

#### String/Filter Style {#string-filter-style}

Separate modifiers with ` | ` pipe delimiters and go. Modifiers can have parameters that alter their behavior, let you target another variable, and do other similar things. Those are delimited by `:` colons.

This style is used for string variables and when writing [conditions](#conditional-logic) (whether the variable is a string or an array).

```
{{ string_variable | modifier:param1:param2 | modifier_2 }}
```

#### Array/Parameter Style {#array-parameter-style}

Modifiers on array variables are formatted like "parameters". You can't use them in conditions when you format them this way. Parameters are separated with pipes instead of colons with this format.

```
{{ array_variable modifier="param1|param2"}}
  // Neat stuff happening here
{{ /array_variable }}
```

#### Full Disclosure... {#full-disclosure}

You can actually use Array style on strings, but String Style on arrays is really annoying because you have to re-apply the modifiers on the closing tag and ain't nobody got time for that. Best to keep the approaches separate as much as you can, for standards.

```
// This is ugly and annoying.
{{ variable|limit:2 }} {{ /variable|limit:2 }}
```

## Conditional Logic {#conditional-logic}

If this, then that. Do this unless something else. _If chicken wings then party_. Statamic has a full set of conditional operators to let you build as much logic as you need into your templates.

Conditions come in two basic flavors, `if/else` and `unless/else`. They serve the same purpose but are logical mirrors of each other. Where `if` only renders when a condition matches, `unless` renders _unless_ it matches. Use whichever makes your templates easiest to read.

```
{{ if }} {{ /if }}
{{ if }} {{ endif }} # your choice of closing tags (be consistent!)

{{ if }} {{ elseif }} {{ else }} {{ /if }}

{{ unless }} {{ /unless }}
{{ unless }} {{ else }} {{ /unless }}
```

Conditions are converted to, and therefore behave just like, PHP expressions. You can use any of PHP’s [comparison][comparison] and [logical][logical] operators.

### Conditions on Tags vs Variables

When working with [tags](/tags) instead of variables, you **must** wrap the tag in a pair of additional single braces to tell the parser to run that logic first.

```
{{ if {form:errors} }} // yep, there are form errors {{ /if }}
```

### Truthiness and Existence {#truthiness-and-existence}

A condition is considered "truthy" when variable exists, is not `null`, and is not `false`.

```
{{ if header }}
    <img src="/img/pelé.jpg">
{{ /if }}

# If all are truthy
{{ if bears && beets && battlestar_galactica }}
  You are faster than 80% of all snakes.
{{ /if }}

# If any are truthy
{{ if bears || beets || battlestar_galactica }}
  Is that you, Mose?
{{ /if }}
```

You can also check if variables don't exist, aka "falsy".

A condition is considered "falsy" when variable doesn't exist, is `null`, or is `false`.

```
{{ if ! soup }}
  So sad.
{{ /if }}

{{ unless soup }}
  Still totally and equally sad.
{{ /unless }}
```

### Matching {#matching}

```
{{ if meal == "breakfast" || meal == "dinner" }}
  <img src="/img/omnomnom.gif">
{{ elseif meal == "fourthmeal" }}
  <img src="/img/chihuahua.jpg">
{{ /if }}
```

### Comparing {#comparing}

```
{{ if bad_guys < 10 }}
  Thow away that shield. You got this.
{{ /if }}

{{ if age >= 21 }}
  You can stay, but no nae nae, kk?
{{ /if }}
```

### Variable Fallbacks {#variable-fallbacks}

When all you need to do is display a variable and set a fallback for when it's falsy, use the `or` shorthand. The following two code blocks are functionally identical. Which do you prefer?

```
{{ meta_title or title or "Pretty little trees" }}
```

```
{{ if meta_title }}
  {{ meta_title }}
{{ elseif title }}
  {{ title }}
{{ else }}
  "Pretty little trees"
{{ /if }}
```

## Combining Modifiers {#combining-modifiers}

You can use [modifiers][modifiers] inside your conditions if you wrap them them up along with their variable in `( parentheses )`.

```
{{ if (date | modify_date:+73 years | format:Y) == "2015" }}
  This was indeed written in 1942. Somehow.
{{ /if }}
```

### Regular Expressions {#regular-expressions}

RegExes aren't for the faint of heart, but if you're capable of [writing one](https://regex101.com/) (or let's face it, copying and pasting from Stack Overflow), you can use the `~` squiggle operator to get the job done.

```
{{ unless content ~ '/\bbest\b.+\bever/'}}
  # The hyperbole here is at least tolerable. Continue!
  {{ content }}
{{ /unless }}
```

**Tips:**

- You need to escape your regex as a string
- You need to use delimiters at the beginning and end of your regex
- You can use flags at the end of your regex to modify its behavior

## Code Comments {#code-comments}

You can add comments to your templates by wrapping text in `{{# #}}`. Anything within these special comment braces will not be shown at all in your rendered source code.

```
{{# I hate pants. #}}
```

## No Parse {#no-parse}

You can prevent the Statamic template parser from parsing chunks of code by wrapping it with `{{ noparse }}` tags. In fact, this very page uses that feature.

```
{{ noparse }}
  Welcome to {{ fast_food_chain }},
  home of the {{ fast_food_chain_specialty_item }},
  can I take your order?
{{ ⁄noparse }}
```

You can also prevent single variables from being parsed by prefixing them with an `@` symbol.

```
Welcome to @{{ fast_food_chain }},
home of the @{{ fast_food_chain_specialty_item }},
can I take your order?
```

## Tags {#tags}

Tags (as a proper noun) are tags that _do stuff_. Depending on the Tag, that can mean different things. Here's a few examples:

- The [Collection Tag](/tags/collection) gets entries from a collection and makes their variables available for you to loop through.
- The [Env Tag](/tags/env) outputs values from your `.env` file.
- The [Redirect Tag](/tags/redirect) doesn't even output anything! It performs a redirect.

Tags can potentially look exactly like [variables](#variables), or at least very similar. Because of this, we've written
an article explaining [the difference between them][tags-vs-vars] in more detail.

### Variables Inside Parameters {#vars-in-params}

Whenever you need to use variables _inside_ of a Tag's parameters, you should use **single** tag braces.

```
{{ nav from="{segment_1}" }}
```

Alternatively, you can prefix the parameter with a `:` and it'll look for the value in the current context (usually the front matter of the thing you're looping through)

```
{{ nav :from="segment_1" }}
```

### Modifiers Inside Parameters {#mods-in-params}

Using a variable indside of a Tag is nice; using a variable with a modifier inside of a Tag is better. And it works exactly as you'd expect...with one small caveat: When using a modifier inside of a Tag, _no whitespace is allowed between variables, pipes, and modifiers_.

In other words:

```
{{ partial:hero image="{ featured_image|url }" }}
```

...Is fine and dandy. But this:

```
{{ partial:hero image="{ featured_image | url }" }}
```

...Will pass the string `{ featured_image | url }` to your partial instead of the url of your image. Much less dandy. Not fine at all.

So remeber to always snug things up next to each other inside your single tag braces.

## Resources {#resources}

If you use [Sublime Text](https://www.sublimetext.com/) or [Atom](https://atom.io/) as a code editor, you can use the [Antlers Syntax Package](https://github.com/addisonhall/antlers-statamic-sublime-syntax) to enable code highlighting. It's lovely! Even [VS Code](https://code.visualstudio.com/) has support with [Statamic Antlers Templates
](https://marketplace.visualstudio.com/items?itemName=ahdesign.language-antlers).

[guide-yaml]: /yaml
[comparison]: http://php.net/manual/en/language.operators.comparison.php
[logical]: http://php.net/manual/language.operators.logical.php
[modifiers]: /modifiers
[tags-vs-vars]: /knowledge-base/tags-vs-variables
