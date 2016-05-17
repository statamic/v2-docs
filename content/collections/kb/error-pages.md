---
title: How to customize error pages
id: cf0640c7-6ca0-4e57-8929-b8815a149084
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
---
Out of the box, Statamic will show its own error pages. For example, if a user were to go snooping where they shouldn't, they may come across a 403 "Access Denied" page.

Error pages will look nice, but often you will want to customize how they look.

Statamic will look inside your theme's `templates/errors`[^1] folder for a template matching the response code. Page not found? That's a 404 error, so a `404.html` template will be loaded.

Sometimes just changing the template isn't enough. If a 403 error is shown, there's a good chance you don't want your navigation to be visible anymore. Statamic will use an `error.html` layout, if one exists. Otherwise, it'll
just load the layout specified in `default_layout` (`default`, by default).

[^1]: This can be adjusted by editing the `error_template_folder` variable in your `theming` settings. For example, set it to `/` if you want your error templates in your root template folder.
