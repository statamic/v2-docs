---
title: Yield
overview: 'Output a section of template that was captured with a [Section Tag](/tags/section).'
id: 4e704127-320e-4580-a948-375f4e40312d
---
## Example {#example}

Here's an example template:

```
<h1>{{ title }}</h1>
<div>
    {{ content }}
</div>

{{ section:sidebar }}
    Some sidebar stuff!
{{ /section:sidebar }}
```

and here's an example layout:

```
<html>
<body>
    <header>...</header>
    
    <section>
        {{ template_content }}
    </section>
    
    <aside>
        {{ yield:sidebar }}
    </aside>
</body>
</html>
```

Everything within the `section:sidebar` tag pair will _not_ be rendered in `template_content`. Instead, it will be 
rendered by the `{{ yield:sidebar }}` tag.

## Fallback content {#fallback-content}

If a section has not been created, the yield tag may contain fallback content by using a tag pair.

In the example above, let's assume that there was no `{{ section:sidebar }}` used in the template.

```
<aside>
    {{ yield:sidebar }}
        This text will be shown since section:sidebar was never used!
    {{ /yield:sidebar }}
</aside>
```



[yield_tag]: /tags/yield
