---
title: Theming
id: 450395e2-f309-4ecb-8f52-cbf4d27870ff
overview: |
  A theme is the perfectly fitting custom tailored suit or dress that your website wears. It can be one of a kind or shared with the masses. What you do with your theme is up to you.
---

## Anatomy of a theme {#anatomy}

A theme is a collection of files and resources that dictate the look and feel for a Statamic site. Templates, CSS, JavaScript, images, and so on. These files are organized into a specific folder structure to ensure portability and compatibility.

### Folder Structure {#folder-structure}

A basic theme will have these primary folders:

``` .language-files
site/
`-- themes/
    `-- your-theme/
        |-- css/
        |-- js/
        |-- layouts/
        |-- partials/
        `-- templates/
```

#### css/ {.icon.icon-folder}

This is where your theme’s CSS files go. Although this folder isn’t required to use Statamic, using it will let you use the
[theme CSS tag](/tags/theme-css).

#### js/ {.icon.icon-folder}

Your theme’s JavaScript files go here. Like the css folder, this folder isn’t required, but files within it will become
available through the [theme JS tag](/tags/theme-js).

#### layouts/ {.icon.icon-folder}

This folder contains your site’s [layouts](#layouts). A theme must have at least one layout file to work. Most themes will
only need one or two layouts at the most.

#### partials/ {.icon.icon-folder}

Partials are small, reusable chunks of code (think embeds or includes in other systems). All of those live here. You
can access your theme’s partials through the [partial tag](/tags/partial).

#### templates/ {.icon.icon-folder}

This folder is where your theme’s [templates](#templates) live. A theme must have at least one template file to work.

#### Other Folders

A theme can contain any number of other folders as well. Beyond the core folders, the rest is up to you. We just recommend following a few [naming conventions](#conventions). As a rule of thumb, your theme should contain all files needed to render your design, and from there any additional resources would be considered "Assets" and part of the site content.

Other folders you might see in a theme from time to time: `scss`, `fonts`, `images`, `less`, `coffee` and so on.


## Installing a Theme {#installing}

Installing a packaged theme is a simple process:

1. Drop the theme folder into your `site/themes` directory
2. Change the active theme by heading to Theming settings in the Control Panel, or by changing `theme`
   in `settings/theming.yaml`.
3. Refresh your site and you should see the new theme.

### When Switching Themes {#when-switching-themes}

A theme contains all of the layouts and templates that your content will use. They’re mostly plug-and-play, but if you’re switching your site from one theme to another, there’s something you must remember: Statamic is open-ended on
what you name things. The templates that come with a new theme might not match those of your old theme.

If that’s the case, you have two options:

- Update your content files to use the correct new theme name
- Change the theme’s template file names to match the ones used by your content


## Conventions {#conventions}

[Theme tags](/tags/theme) assume a few conventions that will be beneficial for you to follow. If you don't care about them or have a specific reason you simply must ignore them, you're free to do your own thing. Just remember that conventions are there to help, not to hinder. Unlike asthma. Conventions are basically the philosophical opposite of asthma.

### CSS and JavaScript {#css-js-conventions}

Stylesheets and scripts should be placed into a `css` folder and a `js` folder (respectively) at the root of your theme’s folder. Feel free to compile, concatenate, or otherwise generate files _into_ these folders. We do it and so should you.

#### Primary Stylesheet

Your main stylesheet should be named the same as your theme. If your theme is called _Linguine_ and lives in a `linguine` folder, your main stylesheet should be named `css/linguine.css`.

#### Primary JavaScript Script

Your main script file should also be named the same as your theme. `{{ theme:js }}`is going to be look for `js/linguine.js`.

### Includes, Embeds, or Partials {#partials}

If you’ve ever used platforms other than Statamic (and if you haven't, go and do so! It's good to be a well rounded individual), you likely have seen other name for “small, reusable chunks of code.” We've seen `includes`, `embeds`, `partials`, and even `snipplrs`. Well, we decided to call them [partials][partials], and we recommend that you do too. At least when building sites with Statamic. Don't confuse people on purpose, that's just mean.


## Layouts {#layouts}

A layout is the markup foundation of your theme. When you create a site there are usually chunks of HTML that are always present (like a nosey neighbor) no matter what page you’re on. Stuff like `doctype`, `<head>`, `<body>`, styles & scripts, and often even your header and/or footer. In Statamic, these can all go in your layout.

By default Statamic will look for a layout called `layouts/default.html`. You can name your layouts (if you need more than one) whatever you’d like, but if you choose a name other than `default` for your default layout, you’ll need to set it explicitly wherever you want to use it.

### If You Think in "Headers & Footers"

Some other systems force you create a header partial, a footer partial, and then
make you manually include them in every page template throughout your site. Layouts works similarly _in concept_, but rather in reverse.

Let's try an analogy, shall we? The Layout is like an intricate, gold-embossed picture frame, and the Template is the beautiful oil painting of your mother-in-law that you place inside the frame.

### Rendering That Template {#placing-page-content}

Now you can put the `{{ template_content }}` tag anywhere in your Layout and Statamic will replace it with your template injected full of your current page’s data. A simple example might look like this:

```
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <title>My Page</title>
    </head>
    <body>
        <header>
            <img src="logo.png" alt="Logo">
            <h1>My Wonderful Site</h1>
        </header>

        {{ template_content }}

        <footer>
            <h2>My name is John and this is my page.</h2>
            <h3>Copyright as such</h3>
        </footer>
    </body>
</html>
```

Aside from the complete output of your template using `template_content`, it's also possible to inject or 
["yield" additional sections](#yielding-sections) from your template into your layout.

### Choosing a Layout {#choosing-layouts}

#### Per Page

To tell one page of your site to use a specific layout, set `layout` in your page’s front-matter. For example, if you’re using the `default` layout in most of the site but want something different for your `promotion` page, that page’s front-matter might look like this:

``` .language-yaml
---
title: Special Marketing Promotion
layout: landing_b
---
```

This will use the code from `landing_b.html` within your current theme’s `layouts` folder. If that file doesn’t exist, Statamic will graciously display an error.

#### Per Folder

If you want to change the layout for an entire folder of your site, you can set `layout` in the `folder.yaml` file of that folder. Your folder’s page file might look like this:

```
layout: upsidedown
```

Note that you can still override the per-folder layout by using the per-page layout method above. We call this pattern a cascade. You'll find it everywhere in Statamic.

### Common Layouts {#common-layouts}

Most sites only need one or two layouts:

- The main site design
- An empty for RSS feeds

## Templates {#templates}

A template is the way one type of page is organized, and how page content is displayed within it.

Templates sit between [layouts](#layouts) and page content. Where a layout is the outer-shell of your website that doesn’t change page-to-page, templates are the inner-shell that tells Statamic how to render the content for a page.

Statamic replaces the `{{ template_content }}` tag in your layout with the chosen template for the page being viewed (and likewise, within that template, Statamic replaces each [tag][templating] with the appropriate value from
the page’s content).

### An Example of Templates {#template-examples}

You’ll usually create a template for each type of page that you want to display. A list of templates used by a standard blog might be:

- home page template
- blog post detail page template
- category list page template
- general static content page template

With this list of templates, we have all of the ways content will be displayed on our site. Each page is told which template to use to correctly display the page’s content.


### Naming and Choosing Templates {#naming-templates}

Naming templates works exactly the same as [naming layouts](#choosing-layouts), but in the `templates` folder.

You also choose templates in the same way as with layouts (per-page and per-folder), except – you guessed it – `template` instead of `layout`.

As you will likely have more templates than layouts, you can feel free to organize your templates folder however you like. If you want to reference a template located in a subfolder, just specify the folder along with the name.

For example:

``` .language-yaml
template: blog/index
```

``` .language-files
your-theme/
`-- templates/
    `-- blog/
        `-- index.html
```


### Variables, Tags, and Template Languages {#template-languages}

Statamic lets you pick between two different templating languages. Not to make things confusing, but to give you more control.

The first and primary choice is [our own language, "Antlers"][templating]. All code examples throughout the documentation assume you're using Antlers. Antler templates are indicated with a `.html` extension on your layout/template/partial file.

We also support [Laravel's Blade][blade] syntax by using the `.blade.php` file extension. For example:

``` .language-yaml
template: one
```

``` .language-yaml
template: two
```

``` .language-files
site/themes/your-theme/
`-- templates/
    |-- one.html
    `-- two.blade.php
```

Be careful not to name overlap names when using both Antlers and Blade templates. Antlers will win every time.

Note that when using Blade, all templates will be located in the `templates` folder. This includes files used by the `@extends` and `@include` directives.

[Read more about templating in Statamic using Laravel Blade](/knowledge-base/blade).

## Yielding Additional Sections in Layouts {#yielding-sections}

As mentioned above, the contents of your template will be injected into your layout with the `template_content` variable.

However, if you'd like to output additional sections, you may use a combination of the [Section](/tags/section) and 
[Yield](/tags/yield) Tags. If you've used Laravel's Blade language, this should feel very familiar to you.

Anything placed between a `section:[name]` tag pair will be excluded from the template, and available in the
corresponding `yield:[name]` tag.

A common scenario for using this feature is to add page-specific Javascript. Here's an example of a contact page that
has a JS-driven form on it. `contact.js` holds the code for the form, but we aren't interested in it being loaded
everywhere - just on the contact page.

```
<h1>{{ title }}</h1>
{{ content }}

<div class="contact-form">
    ...
</div>

<!-- everything between these tags will disappear from your template... -->
{{ section:page_scripts }}
    <script src="contact.js"></script>
{{ /section:page_scripts }}
```

```
<html>
<body>
    <header>...</header>
    
    <section>
        {{ template_content }}
    </section>
    
    <script src="main.js"></script>
    
    <!-- ...and be output over here! -->
    {{ yield:page_scripts }}
</body>
</html>
```

## Best Practices {#best-practices}

Here are a few good ideas to consider when building a theme, some of them a summary in case you skimmed this long boring page.

- Name your main CSS and JavaScript files the same name as your theme
- Template, layout, and partial files should use `.html` for Antlers and `.blade.php` for Blade.
- Only put image files pertaining to your theme in your theme directory; content-related imagery should go in [asset containers][assets] to properly maintain separation of content from presentation
- Create as many templates and subdirectories as you’d like. Group them logically.
- Use relative paths in your CSS files, for example: `url(../img/bg.png);`
- Use lowercase filenames
- When sharing, distributing, or selling a theme, feel free to include a directory for `site/content` showing sample posts and pages that use your theme’s templates, etc.


[blade]: http://laravel.com/reference/blade
[assets]: /assets
[templating]: /antlers
[partials]: /tags/partial
