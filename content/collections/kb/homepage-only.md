---
title: Why can I only see the homepage?
kb_categories:
  - Troubleshooting Common Scenarios
id: 3365a1e0-4a1c-4cc5-8ed3-aada1ee81e2d
---
The number 1 reason for this is because you don't have your URL rewrites set up.

Out of the box, Statamic will omit `index.php` from URLs, because it's the "right" way to do things. That also means
we assume you will have URL rewrites set up in one way or another.

If you're running Apache, you're probably missing an `.htaccess` file. If you're running Nginx, you haven't set up
rules in your `nginx.conf.` Statamic comes bundled with sample versions of these files.

For more details, check out the [URL rewrite manual page](https://docs.statamic.com/installing#rewrites)
