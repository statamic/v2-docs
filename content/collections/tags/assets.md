---
id: 5b748a3f-be0e-41c1-8877-73f6b7ee1d0a
title: Assets
overview: >
    Used to retrieve Assets data from the [Asset Fieldtype](/docs/fieldtypes/assets). Your site's Assets are managed and stored independently of your pages and entries and have a joined relationship through their `id`. [Learn more about Assets](/guides/assets).
variables:
  -
    name: id
    type: string
    description: The ID of the asset
  -
    name: title
    type: string
    description: "The title, if set."
  -
    name: url
    type: string
    description: "The URL to the asset."
  -
    name: path
    type: string
    description: "The relative path from the asset container."
  -
    name: basename
    type: string
    description: "The filename. No path, but with the extension. eg. `jedi.jpg`"
  -
    name: filename
    type: string
    description: "The filename. No path, no extension. eg. `jedi`"
  -
    name: extension
    type: string
    description: "The file extension. eg. `jpg`"
  -
    name: size
    type: string
    description: "A human readable version of the filesize. It will be displayed in the most appropriate format. eg. `36b`, `125KB`, `20MB`, `1.8GB`"
  -
    name: size_bytes
    type: string
    description: "The filesize, in bytes."
  -
    name: size_kilobytes
    type: string
    description: "The filesize, in kilobytes."
  -
    name: size_megabytes
    type: string
    description: "The filesize, in megabytes."
  -
    name: size_gigabytes
    type: string
    description: "The filesize, in gigabytes."
  -
    name: size_b
    type: string
    description: "The filesize, in bytes."
  -
    name: size_kb
    type: string
    description: "The filesize, in kilobytes."
  -
    name: size_mb
    type: string
    description: "The filesize, in megabytes."
  -
    name: size_gb
    type: string
    description: "The filesize, in gigabytes."
  -
    name: last_modified
    type: string
    description: "The time the file was last modified, as a string formatted by whats defined in your config. eg. `January 18th, 2015`"
  -
    name: last_modified_timestamp
    type: string
    description: "The time the file was last modified, as a timestamp."
  -
    name: last_modified_instance
    type: string
    description: "The time the file was last modified, as a `Carbon` instance."
---
## Usage {#usage}

The most basic usage is to iterate over an array of asset IDs. The tag takes its second segment from the name of the variable you wish you connect to. For example, if you have an `images` field, you would use `{{ assets:images }}`.

Here's an example of some Assets in use.

``` .language-yaml
bacon_images:
  - 89jf89r32-fdsmar39ifm9-932c9m3j3
  - 2n329ofna-90fjqp49j9fr-e98rjaa82
```

```
{{ assets:bacon_images }}
  <img src="{{ url }}" alt="{{ alt }}" /> Size: {{ size }}
{{ /assets:bacon_images }}
```

``` .language-output
<img src="/assets/applewood-smoked.jpg" alt="Applewood" /> Size: 355kb
<img src="/assets/canadian-bacon.jpg" alt="Canadian" /> Size: 125kb
```

### Single Assets

If you have an asset field with `max_items: 1` the data will be saved as a `string`. As one cannot iterate over a string, the tag will adjust accordingly without complaining.

``` .language-yaml
hero_image: f9ruqp-pjo32n43ojn3-rijo3209
```

```
{{ assets:hero_image }}
<img src="{{ url }}" />
{{ /assets:hero_image }}
```

``` .language-output
<img src="/assets/img/superman.jpg" />
```

### Asset (singular) Tag

You may have noticed that "Assets" is plural.  If you have an array of assets and only want the first or if it bothers you that a plural-powered Tag would return a single asset, we have you covered. We also support the singular word `Asset` for the explicit purpose of only ever accessing a single Asset.

``` .language-yaml
hero_image: f9ruqp-pjo32n43ojn3-rijo3209
```

```
{{ asset:hero_image }}
<img src="{{ url }}" />
{{ /asset:hero_image }}
```

``` .language-output
<img src="/assets/img/quailman.jpg" />
```
