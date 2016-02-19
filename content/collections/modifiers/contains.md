---
types:
  - condition
id: 75145be0-966f-490e-af3d-ed122eb6445b
---
Search a string and return `true` if match found, otherwise `false`. Case-insensitive by default but can be made sensitive by setting the second parameter to `true`.

```.language-yaml
summary: "It was the best of times, it was the worst of times."
```

```
{{ if summary | contains:BEST }}
{{ if summary | contains:BEST:true }}
```

```.language-output
true
false
```