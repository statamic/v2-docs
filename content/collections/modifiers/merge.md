---
types:
  - array
id: d15bda69-36ee-4871-82b2-e66447868643
---

Merge an array variable with another array variable.

```.language-yaml
fruit:
  - apples
  - bananas
  - bacon
meat:
  - pork
  - beef
  - chicken
```

```
{{ fruit }}
```

```.language-output
apples bannanas bacon
```

```
{{ meat }}
```

```.language-output
pork beef chicken
```

```
{{ fruit merge="meat" }}
  {{ value }}
{{ /fruit }}
```

```.langauge-output
apples bananas bacon pork beef chicken 
```