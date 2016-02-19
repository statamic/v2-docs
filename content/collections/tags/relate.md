---
title: Relate
overview: |
  Relate allows you to fetch content from a relationship using a very simple tag syntax. Relate tags can even be nested inside each other.

  It is designed to work hand in hand with the Relate field.
id: dd9513b4-4cbb-4481-bf72-eb076e053b04
---
## Parameters {#parameters}

The parameters are inherited from the [collection](/docs/tags/collection) tag. Everything available there is available here.

## Example {#example}

Let’s say you have a blog post with a Suggest field called `similar_posts` that you use to set, you guessed it, similar posts. That relationship data is saved as a YAML list of IDs, like so:

``` .language-markdown
---
title: Why Bears are Awesome
similar_posts:
  - 892hnfe983f
  - 980naf80d9a
---
Bears. Beets. Battlestar Galactica.
```

You'd then use the `relate` tag to fetch field data from those entries with a single tag.

```
<ul class="similar-posts">
  {{ relate:similar_posts }}
      <li><a href="{{ url }}">{{ title }}</a></li>
  {{ /relate:similar_posts }}
</ul>
```
