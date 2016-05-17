---
title: How does the "Variable Cascade" work?
id: 4bd7f0a0-b339-4052-8e9c-d231fc339452
---
Statamic's unqiye data structure gives you the ability to set and override data, variables and settings alike, as you traverse deeper into your file structure.

To best illustrate the Cascade, we'll use the example of a blog post that contains a list of related posts.

<div class="screenshot" markdown="1">
![](/assets/examples/cascade-post.jpg)
</div>

Here we can see a number of scopes, outlined beautifully with multicolored lines.

Firstly, the outermost scope (blue) will contain your Global variables.

Then heading inwards (green), we arrive at the `page` scope. This level will contain variables associated with
data at the current URL. In our example, we're on the "My First Day" entry's URL. This means that all the
variables inside that entry will be available. Need the title? Use `{{ title }}`. There's no need to use something
like the `{{ get_content }}` tag to retrieve the data manually.

This is also the first time you may see the cascade at work. Let's say there's a global variable named `overview`.
If your entry also contains an `overview` variable, the global will be overridden. Depending on the situation,
this could be beneficial or frustrating. [We'll learn how to combat this later](#fighting-the-cascade).

Moving further inwards, we have two Tags being used. A Relate tag for taxonomy terms (the terms being represented by
purple outlines), and a Collection tag (entries in red).

Both of these would follow the same rules of the Cascade. Within either of these loops, their variables would override
the parent. For example, the entries (red) display their titles using `{{ title }}`. Inside the first red block,
`{{ title }}` would output `Speeder Bikes, Wookies, and Ewoks`, where outside that loop, `{{ title }}` would output
`My First Day`.

## Fighting the Cascade {#fighting-the-cascade}

More often than not, the Cascade is a wonderful thing. However, sometimes it hinders us.

The most classic example is dealing with missing fields when inside a loop.

In our example, the `page` contains a `tags` field for showing taxonomy terms. If we were to try to use the `tags` field
in the collection loop (red), and one of the entries didn't have a `tags` field, it would actually use the `tags` from
the `page` scope.

To ensure that the collection tag only ever uses variables from the entry in the current iteration of the loop, we can
explicitly "scope" the variables.

```
{{ collection:blog scope="post" }}
    <div class="post">
        {{ post:title }}
    </div>
{{ /collection:blog }}
```

The other popular example of fighting against the cascade is when you _want_ to access a parent variable. For example,
when you're in a collection loop and you'd like to get a value in the `page` scope.

Well, variables in the `page` scope _are_ actually aliased into the `page` variable. So at any point, you can access
page level variables by doing `{{ page:variable_name }}`.

```
<h1>Our trip to {{ title }}</h1>

{{ collection:gallery location:is="{title}" }}
    <img src="{{ image }}" alt="{{ title }} in {{ page:title }}" />
{{ /collection:gallery }}
```

Here we have a page where the title is where we went on vacation last summer. `Italy` sounds good. Then we loop over any
gallery entries where the location is in Italy. Within each image we want the alt tag to say what happened and where.
The gallery entry might have its own `title` of the event (`Visiting the Colosseum`) and then append `in Italy`.


## Reserved variables {#reserved}

To help in wrangling the cascade, we've aliased variables into other places. The main example is the `page` array. If
you were to create a variable named `page`, that would override the `page` scope. Don't do that.

Here's a list of words we recommend that you don't use as field/variable names.

- page
- global / globals
