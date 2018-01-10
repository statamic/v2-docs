---
title: Bard
description: "Bard is designed for rich post layouts and maxmium flexibility."
overview: |
  <span class="highlight">Bard is designed for rich article layouts</span>. It's more than a content editor, it's practically a layout designer. Bard starts with simple, rich text editor with popup formatting controls. It stores structured data and adds the ability to insert blocks of any arrangement of custom fields amidst the text.

  It's a lot like [Medium's](https://medium.com) editor, but for your own site. It's also 100% compatible with [Replicator's](/fieldtypes/replicator) data structure — you can easily switch between their interfaces if you desire.
image: /assets/fieldtypes/bard.gif
options:
  -
    name: allow_source
    type: boolean _true_
    description: Controls whether the "show source code" button is available to your editors.
  -
    name: sets
    type: array
    description: An array containing sets of fields.
  -
    name: buttons
    type: array
    description: |
      An array of buttons that should appear in the toolbar. You can choose from `bold`, `italic`, `anchor`, `h2`, `h3`, `quote`,
      and any [custom buttons][custom]. By default, all of them are displayed.

      [custom]: #custom-buttons
  -
    name: markdown
    type: boolean *false*
    description: |
      _Experimental_. Enabling this option will allow you to author (and save) Markdown. The toolbar, toggle source button, and keyboard shortcuts will be disabled.
added_in: 2.8
id: f4bf58d3-cbce-4957-b883-d92fd4791e89
---
## Usage

At first glance, Bard looks and feels like a simple text editor, but once you start writing you'll realize there's much more to it.

**Clean HTML**: Bard writes clean HTML. It's only allowed a very basic set of tags and aggressively strips anything out. This isn't WYSIWYG. This is better. This is structured content. There's even a code mode so you can take a peek and be sure.

**Formatting Options**: Highlight some text and you get the most commonly used formatting options: bold, italic, link, h2, h3, and blockquote.

**Custom Field Blocks**: Between text paragraphs you can insert blocks of custom content. Most commonly this would be images or videos, but it can be quite literally any combination of one or more [available fieldtypes][fieldtypes] grouped into sets. Just like [Replicator][replicator].

**Drag and Drop Reordering**: Want to rearrange your post layout? Move the images around? Bring your TL;DR from the bottom up to the top? Just drag those blocks around.

**Full Screen Mode**: Like to cut out all possible distractions while you write? No problem.



## Data Structure {#data-structure}

Bard stores your data as an array, exactly like [Replicator][replicator]. Sensing a theme? The default text content will be scoped into `type: text` as HTML content.

```.language-yaml
bard_field:
  -
    type: text
    text: <p>I love it when he plays the saxophone. It gives me all the feels.</p>
  -
    type: image
    image: /assets/img/sax.jpg
    caption: "I want this mounted above my fireplace."
```

### Text-only Mode

If you don't configure any additional sets the field data will be saved as a string.
```.language-yaml
bard_field: "<p>Oh hi Mark.</p>"
```

> Please note that you **cannot** use a Bard fieldtype for the `content` field.

## Templating {#templating}

Use tag pair syntax with `if/else` conditions to style each set accordingly.

```
{{ bard_field }}

  {{ if type == "text" }}

    <div class="text">
      {{ text }}
    </div>

  {{ elseif type == "image" }}

    <figure>
      <img src="{{ image }}" alt="{{ caption }}" />
      <figcaption>{{ caption }}</figcaption>
    </figure>

  {{ /if }}

{{ /bard_field }}
```

An alternative approach (for those who like DRY or small templates) is to create multiple "set" partials and pass the data to them dynamically, moving the markup into corresponding partials bearing the set's name.

```
{{ bard_field }}
  {{ partial src="sets/{type}" }}
{{ /bard_field }}
```

```language-files
partials/
|-- sets/
|   |-- gallery.html
|   |-- image.html
|   |-- poll.html
|   |-- text.html
|   |-- video.html
```

## Custom Buttons {#custom-buttons}

You can create custom buttons by creating a [MediumEditor button extension](https://github.com/yabwe/medium-editor/blob/master/src/js/extensions/WALKTHROUGH-BUTTON.md).
You can load the Javascript either through an addon's `scripts.js`, or by your own `site/helpers/cp/scripts.js`.

Here's an example of a button that prompts you for a link:

``` .language-yaml
fields:
  story:
    type: bard
    buttons:
      - bold
      - askForLink # corresponds to the "name" of the extension
```

``` .language-js
var AskForLinkButton = MediumEditor.extensions.button.extend({
    // Should match what you add to the Statamic object below, and the button name.
    name: 'askForLink',

    tagNames: ['a'],
    contentDefault: '<span class="icon icon-bug"></span>',
    aria: 'Ask For Link',

    handleClick: function () {
        const href = this.getHref();
        if (!href) return;

        const selection = this.base.options.contentWindow.getSelection().toString().trim();
        const html = `<a href="${href}">${selection}</a>`;
        this.base.pasteHTML(html);

        this.base.checkContentChanged();
    },

    getHref: function () {
        // Get the href from a modal, etc.
        return prompt('Enter the link?');
    }
});

// Add it to the Statamic object.
Statamic.MediumEditorExtensions.askForLink = new AskForLinkButton;
```

[replicator]: /fieldtypes/replicator
[fieldtypes]: /fieldtypes
