---
title: Users
overview: Grab, filter, and iterate over Users and their profile data.
parameters:
  -
    name: sort
    type: string *username*
    description: Sort entries by a field.
  -
    name: group
    type: string
    description: Specify the slug to filter users by a single group.
  -
    name: role
    type: string
    description: Specify the slug to filter users by a single role.
id: c3e6df36-d705-4293-a5d8-40d27a06a18c
---
This tag has the same functionality as the [collection](collection) tag, with some differences.

The first big and obvious difference, is that the collection you're working with is all of your users.

## Example {#example}

Here we will loop over the users with gmail email addresses.

```
<ul>
  {{ users email:ends_with="@gmail.com" }}
    <li>{{ username }}</li>
  {{ /users }}
</ul>
```

[collection]: /tags/collection
