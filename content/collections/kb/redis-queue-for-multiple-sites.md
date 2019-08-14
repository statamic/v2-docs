---
title: Using a Redis queue for multiple sites on one server
overview: You can run a Redis queue to optimize performance in the Control Panel when using Spock. With a Redis Queue you can also push other tasks, like generating assets, into the background. This article explains how you can make a Redis queue work for multiple Statamic sites on one server.
kb_categories:
  - Tips, Tricks, and How-Tos
related_reading:
  - 302be45f-bd9d-4a26-b836-d0f5c84b4773
  - ec91d09e-c5a4-447e-b5a2-fd6ee2d7f89a
id: 03924f80-71cf-11e9-b475-0800200c9a66
---
## Enable Redis queue
You can enable a Redis queue for your website by setting the following in your `.env` file:

```.language-env
QUEUE_DRIVER=redis
```

For each site on your server, you want Redis to use it's own database, or you'll run into troubles. You can do this by adding the following to your `.env` file. Make sure you increase the database number for each site you're running:

```.language-env
REDIS_DATABASE="0"
```

By default you have 16 Redis databases at hand.

## Run a queue worker for each site

You need to run `php please queue:listen` for each site to activate the queue. If you use Forge you can easily configure a daemon for each of your sites. Configure this in the Daemon tab on the Server Details page of your server on Forge. Use the following configuration:

```
Command: php please queue:listen
User: forge
Directory: /home/forge/YOUR_SITE_NAME/
```

This makes sure the queue automatically starts after a reboot, failure, or a deploy. Happy queueing!
