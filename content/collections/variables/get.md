---
id: 546c4334-df40-4e9a-aff4-a56c43e839d8
types:
  - global
---
An array of sanitized `GET` variables that come from any query strings present in the current URL. It can be used as a tag pair with access to all your parameters or as a single tag to access parameters directly. A counterpart to `{{ post }}`.


Example URL: `/about?show=pants&hide=jeggings`

```
{{ get }}
  {{ show }}
  {{ hide }}
{{ /get }}

{{ get:show }}
```

``` .language-output
pants
jeggings

pants
```
