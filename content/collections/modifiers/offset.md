---
types:
  - array
  - utility
id: 9433b8cd-b2e0-4fbf-85bd-85edf317efa4
---
Offsets the items returned in an array.

```.language-yaml
playlist:
  - Emancipator
  - Gong Gong
  - Possom Posse
  - Justin Bieber
```

```
{{ playlist | offset:1 }}
```

```.language-output
playlist:
  - Gong Gong
  - Possom Posse
  - Justin Bieber
```
