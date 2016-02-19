---
title: Redactor
overview: 'Redactor is a fast, retina-ready WYSIWYG editor fieldtype. It’s lightweight, customizable, and powerful. Currently using [Redactor 10](https://imperavi.com/assets/pdf/redactor-documentation-10.pdf) you can check out the docs to see what options are available to pass in.'
image: 22318543-6610-45b9-b21b-d1bed3b2afe7
options:
  -
    name: settings
    type: array
    description: >
      An array of settings to be passed into
      the Redactor plugin. This should be a
      YAML version of the Javascript object
      you would use to initialize Redactor.
id: 25d8be49-2300-42ac-90e4-92df42fc2906
---
## Data Structure {#data-structure}

By design Redactor saves HTML code.

``` .language-yaml
quote: |
  <blockquote>I signed up for Second Life about a year ago. Back then, my life was so great that I literally wanted a second one. Absolutely everything was the same... except I could fly.</blockquote><p>– Dwight Schrute</p>  
```

This is fine, but keep it in mind if you're using it for your `content` field set to "markdown". Statamic parse the `content` as Markdown and any unintended text indentation will be converted to code blocks.

[redactor-docs]: https://imperavi.com/assets/pdf/redactor-documentation-10.pdf


## Redactor.js Configuration {#configuration}

You are able to customize all of the Redactor options. You can [view the full list of settings in the Redactor documentation][redactor-docs]

Any settings and options available to the plugin can be set here. For example, if the docs say to use the following configuration object:

``` .language-javascript
$('textarea').redactor({
  formatting: ['p', 'blockquote', 'h2'],
  minHeight: 300
});
```

You would translate to YAML like so:

``` .language-yaml
settings:
  formatting:
    - p
    - blockquote
    - h2
  minHeight: 300
```

*Note: Function type options (eg. callbacks) are not supported.*
