---
title: Configuring File Upload Size
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
id: 51f8e44c-7be2-4af0-9ee2-941e2267e10a
---
It's often necessary to modify your server configuration to increase the maximum size of any file uploads. It's annoying, but there's just nothing we can do about it on the application side, aside from displaying a message saying the file is too darn big.

Here's how you do it. Let's assume you want to allow files up to 50 MB in size.

## Apache

If your server allows you to override your PHP config in your ``.htaccess`, you can add the following lines:

```
php_value upload_max_filesize 50M
php_value post_max_size 50M
```

Otherwise, you'll need to edit your `php.ini` like so:


```
upload_max_filesize = 50M
post_max_size = 50M
```

## Nginx

Inside your nginx.conf's `server` block, you'll need to add the following, and then restart Nginx.

```
server {
  client_max_body_size 50M;

  # all that other stuff
}