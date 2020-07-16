---
types:
  - markup
  - string
  - utility
id: 3ef1e731-742e-48c2-81f0-6fa916ecda0a
---

## String
Returns the first X characters of a string, where X is any positive integer.

```.language-yaml
title: 2015 Year Books Photos
```

```
{{ title | first:4 }}
```

```.language-output
2015
```

## Array

Returns the first item of an array.

```.language-yaml
meat:
  - Bacon
  - Steak
  - Fillet
```

```
{{ meat | first }}
```

```.language-output
Bacon
```