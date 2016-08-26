---
title: YAML
id: e31e44d1-a82d-438d-9a8d-5a22fe4b76e3
overview: >
  YAML is a data storage format designed to be human readable and easily manipulated by hand. In fact, it's 100% intercompatible with JSON, just easier to write. We use it extensively to store data, content, and manage settings. YAML all the things.
---

## To the reader... {#to-the-reader}

If you plan to use the Control Panel exclusively to manage content and settings (maybe you're a content manager or had a developer build you a site on Statamic) you probably don't need to know anything about YAML. But wait! You might find it interesting and may even geek out a bit with your newfound knowledge. Admittedly it's less exciting than the new Star Wars trailers, but more than picking out carpet samples for your new partially-finished basement. *Developers: you should read this end to end! **There and back again.***

## So, what exactly _is_ YAML? {#what-is-yaml}

YAML stands for "YAML Ain't Markup Language". A classic example of the [recursive acronym][recursive-acronym]. Originally it stood for "Yet Another Markup Language" but semantically-oriented people quickly shut that down, denoting the fact that nothing was being marked up, but rather data was being structured. So on that fateful day (which was a Wednesday), YAML became self-referential. `</tangent>`.

YAML complies with the JSON spec, making it easy to interchange it with nearly any native data format. It consists of key and value pairs delimited by a colon+space. Always a colon followed by a space. Did you get that?

```language-yaml
variable_name: value
```

YAML is usually stored in `.yaml` or `.yml` files, but can often (and especially in Statamic's case) can be found inside the top of other text files. This is referred to as "YAML Front Matter", or simply "Front Matter", and it looks like this:

```language-markdown
---
title: Hey everyone, this is Front Matter!
---
Some letters strung together in a specific order down here.
```

## Let's YAML Together! {#lets-yaml-together}

Let's explore how to use YAML to structure some of the most common types of data.

### Strings {#strings}

A string is nothing more than a single sequence of characters. These following variable definitions are equivalent:

```language-yaml
my_name: Niles Peppertrout
```

```language-php
$my_name = "Niles Peppertrout";
```

```language-javascript
var my_name = "Niles Peppertrout";
```

Simple, right?

### String Escaping {#string-escaping}

As YAML is a **structured** data format, you will occasionally need to quote your strings to prevent rogue apostrophes, commas, and other reserved constructs from confusing the parser, allowing you to structure your data exactly as desired.

You can also use quotes to force or typecast another datatype as a string, For example, if your key or value is `10` but you want it to return a String and not an Integer, write `'10'` or `"10"`.

```language-yaml
# Probably broken
restaurant: Joe's Mountain Lodge

# Perfection
restaurant: "Joe's Mountain Lodge"
```

### String Block Literals {#string-block-literals}

You may have thought to yourself: "But gentlemen! My string has many lines! And is a whole pile of HTML with lots of quotes, characters, indentation, and tender loving care! What then?" YAML knew your thoughts. Multi-line strings can be written in two ways.

#### Preserving newlines {#preserving-newlines}

You can preserve any line breaks in your string block, which is useful when writing Markdown or preserving HTML line breaks. Add a `|` and then indent the rest of the content. That initial indent will be stripped.

```language-yaml
lyrics: |
  Pierce I don't need you in my band
  I don't need your heart or your hand
  Oh I don't need you in my life
  All you do is cause me such strife
```

#### Collapsing newlines {#collapsing-newlines}

Completely ignore those line breaks with a `>`, and indent the rest of the content. That initial indent will be stripped.

```language-yaml
test: >
  These
  lines will
  be collapsed
  into a single
  paragraph.

  And blank lines are
  paragraph breaks.
```

YAML doesn't stop at string values. Let's go deeper, and as we do it's important to note something important. In fact it's so important we noted its importance with notably incorrect grammar.

> YAML must always use _two spaces_ for indentation. Deal with it. As you were.


### Lists (indexed arrays) {#lists}

Lists can be structured like a plain-text bulleted list or comma delimited by inside brackets, much like JSON.

```language-yaml
shopping_list:
  - sunglasses
  - sandals
  - spinning beach ball

layaway: [aloe vera, winter coat]
```

When parsed, the data will be converted into typical indexed arrays:

```language-php
$shopping_list = [
    [0] => "sunglasses",
    [1] => "sandals",
    [2] => "spinning beach ball"
];

$layaway = [
    [0] => "aloe vera",
    [1] => "winter coat"
];
```

### Named Lists (Associative Arrays) {#named-lists}

```language-yaml
antisocial:
  facebook: http://facebook.com/statamic
  twitter: http://twitter.com/statamic
```

```language-php
$antisocial = [
    ["facebook"] => "http://facebook.com/statamic",
    ["twitter"] => "http://twitter.com/statamic"
];
```

```
<a href="{{ antisocial:facebook }}">Facebook</a>
<a href="{{ antisocial:twitter }}">Twitter</a>
```

### Lists of Lists (Multidimensional Arrays) {#lists-of-lists}

You can create lists of lists, and those lists can have lists, and those lists can have their own lists and lists of lists and -- now you should think carefully before you go any deeper but -- those lists can also have some of their very own lists and lists of lists. Got it?

This is a very common pattern in Statamic. Entries Listings, Grid and Replicator fieldtypes, and Search results all use multidimensional array data.

```language-yaml
students:
  - name: Abed Nadir
    school: Greendale
  - name: Troy Barnes
    school: Greendale
```

You may also see the list items indicators `-` on their own line. Both formats are perfectly acceptable and interchangeable.

```language-yaml
students:
  -
    name: Pierce Hawthorne
    school: Greendale
  -
    name: Jack Black
    school: The School of Rock
```

Look at how pretty that data is.

```language-php
$students = [
    [0] => [
        ["name"] => "Abed Nadir",
        ["school"] => "Greendale"
    ],
    [1] => [
        ["name"] => "Troy Barnes",
        ["school"] => "Greendale"
    ]
];
```

### Mixing and Matching {#mixing-and-matching}

You can build multidimensional arrays full of associative arrays, and vice versa.

```language-yaml
trips:
  vacation:
    - France
    - New Zealand
  work:
    - Fargo
    - Boise
```

### Comments {#comments}

You can comment out any line of YAML by prefixing it with a `#` hash symbol.

```language-yaml
# title: I Quit My Day Job!
title: Another Monday
```

### Casting Data Types {#type-casting}

YAML autodetects the datatype of the entity. Sometimes you'll want to cast the datatype explicitly, like when a single word string that looks like a number or  boolean may need disambiguation by surrounding it with quotes or use of an explicit datatype tag.

```language-yaml
a: 42                     # an integer
b: "42"                   # a string, disambiguated by quotes
c: 42.0                   # a float
d: !!float 42             # also a float via explicit data type prefixed by (!!)
e: !!str 42               # a string, disambiguated by explicit type
f: !!str Yes               # a string via explicit type
g: Yes                     # a boolean True (yaml1.1), string "Yes" (yaml1.2)
h: Yes we have No whiskey  # a string, "Yes" and "No" disambiguated by context.
```

## Conclusion {#conclusion}

So there you have it. YAML in its many shapes and forms. To learn how to render all this lovely data in a template, check out the [Statamic Template Language guide][template-language].

Happy YAMLing!

[recursive-acronym]: https://en.wikipedia.org/wiki/Recursive_acronym
[template-language]: /guides/template-language
