---
title: Logout URL
overview: Generates a link to log a user out.
parameters:
  -
    name: redirect
    type: string
    description: >
       Where the user should be redirected
       after logging out. Defaults to the home
       page.
id: 232b878c-18da-4d01-80f3-a85fcdf65ed8
---
## Example {#example}

```
<a href="{{ user:logout_url }}">Log out</a>
```
