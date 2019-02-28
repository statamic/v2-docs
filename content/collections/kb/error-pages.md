---
title: Customizing error pages
id: cf0640c7-6ca0-4e57-8929-b8815a149084
kb_categories:
  - Tips, Tricks, and How-Tos
---
Statamic will show its own error pages by default. For example, if a user were to go snooping where they shouldn't, they may come across a `403` "Access Denied" page.

But what if you want to customize their appearance?

## Error Templates

Statamic will look inside your theme's `templates/errors`[^1] folder for a template matching the response code. Page not found? That's a 404 error, so a `404.html` template will be loaded.

## Error Layout

If you don't want to use your default layout view (for example if you want to remove your nav, footer, etc) you can create a `layouts/error.html` file.

## Setting Page Variables

The easiest way to set page variables on error templates would be by adding a block of [YAML Front Matter](/yaml) to the top of the template.
 
```
---
title: 404 Ruh Roh
---

<h1>Page Not Found</h1>
<p>How about the <a href="/">home page</a> instead?</p>
```

[^1]: This can be adjusted by editing the `error_template_folder` variable in your `theming` settings. For example, set it to `/` if you want your error templates in your root template folder.

## Templating a 500 error

If you want to hide site visitors from seeing a potential 500 error on the front-end of your site, or show them a nicer error screen, then you can do so by creating a `500.blade.php` file inside `templates/errors`. *Note:* You must have Debug mode turned off in order for this template to show.
