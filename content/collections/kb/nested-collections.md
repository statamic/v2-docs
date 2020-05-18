---
title: Nested collection loops
id: 889f5559-31d2-431a-9710-c09c58f0db51
kb_categories:
  - Tips, Tricks, and How-Tos
---

Sometimes one needs to nest collection loops, for example when implementing a master-detail page. 

The Antlers parser can't handle this form:
```
{{ collection from="blog" }}
  <p>
    {{ collection from="stuff" }}
      <span>{{ title }}</span>
    {{ /collection }}
  </p>
{{ /collection }}
```

But this works :
```
{{ collection:blog }}
  <p>
    {{ collection from="stuff" }}
      <span>{{ title }}</span>
    {{ /collection }}
  </p>
{{ /collection:blog }}
```

And so does this one:
```
{{ collection from="blog|events" }}
  <p>
    {{ partial:stuff_blog }} {{# contains the inner loop code #}}
  </p>
{{ /collection }}
```
