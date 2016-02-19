---
types:
  - string
  - utility
id: cbb290f3-ffd0-4dc1-8989-1d10a92ff17d
---
Attempts to prevent widows (a line with a single word) in a string by adding non-breaking spaces between the last two words of each paragraph.

```.language-yaml
string: I Just Want Pretty Headlines and Sentences
```

```
{{ string | widont }}
```

```.language-output
I Just Want Pretty Headlines and&nbsp;Sentences
```
