---
title: Assets
overview: Upload files and use the Asset Browser to pick from existing files in your Asset Collections.
image: e77d6351-1a1c-4f6b-85e0-ab447e0b3bb6
options:
  -
    name: container
    type: string (id)
    description: >
      `id` of the asset container to connect
      to
  -
    name: folder
    type: string
    description: >
      Optionally choose a folder to restrict files to.
  -
    name: max_files
    type: int
    description: The maximum number of allowed files
id: d0c65546-74f1-4a15-89d5-1562a95ee2c6
---
## Data Structure {#data-structure}

The Asset fieldtype can upload files to existing Containers and stores the `id` of all uploaded and/or selected files.

``` .language-yaml
bacon_images:
  - 89jf89r32-fdsmar39ifm9-932c9m3j3
  - 2n329ofna-90fjqp49j9fr-e98rjaa82
```

## Templating {#templating}

Use the [Assets tag](/reference/tags/assets) to loop through the IDs and fetch the available data.

```
{{ assets:bacon_images }}
  <img src="{{ url }}" alt="{{ alt }}" />   Size: {{ size }}
{{ /assets:bacon_images }}
```

``` .language-output
<img src="/assets/applewood-smoked.jpg" alt="Applewood" /> Size: 355kb
<img src="/assets/canadian-bacon.jpg" alt="Canadian" /> Size: 125kb
```
