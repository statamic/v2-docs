---
types:
  - array
  - utility
id: a3c9e1c5-10ec-44da-b8d8-fdc603fce5a3
---
Limits the number of items returned in an array.

```.language-yaml
playlist:
  - Emancipator
  - Gong Gong
  - Possom Posse
  - Justin Bieber
```

```
{{ playlist | limit:3 }}
```

```.language-output
playlist:
  - Emancipator
  - Gong Gong
  - Possom Posse
```
