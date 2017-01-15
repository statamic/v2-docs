---
title: Users
description: Relate one or more Users to your content.
overview: >
  Allows you attach Users to your content. This can be used to show authorship, team members, or whatever other use you have for showing people with your content.
image: 1732af72-da76-4a23-81fb-69a41b860027
id: 0f8102b9-c948-4264-8cb8-cbfbd0415a04
options:
  -
    name: default
    type: string
    description: >
      The default value. If you specify `current`, then the logged in user will be selected by default.
---
## Data Structure {#data-structure}

The Users fieldtype is a [Relate fieldtype](/fieldtypes/relate), which means the users will be saved as IDs.

``` .language-yaml
jedis:
  - 892jfsd9a90as
  - 134jk1h78dfas
```

## Templating {#templating}

Use the [Relate tag](/tags/relate) to loop through the IDs and fetch the user data.

```
<ul>
  {{ relate:jedis }}
    <li>{{ first_name }} {{ last_name }}</li>
  {{ /relate:jedis }}
</ul>
```

``` .language-output
<ul>
  <li>Luke Skywalker</li>
  <li>Yoda</li>
</ul>
```
