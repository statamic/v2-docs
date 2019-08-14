---
title: Caching with Redis
overview: If you find yourself in a situation with a huge site, small amount of memory, and static caching isn’t an option, using a Redis cache may be a great way to increase site performance.
kb_categories:
  - Tips, Tricks, and How-Tos
related_reading:
  - 102f8c10-9120-4a6e-a630-97050b494b89
  - 0b47d058-a8cc-41ec-9938-b9ef68c03d8d
  - e31e44d1-a82d-438d-9a8d-5a22fe4b76e3
  - 302be45f-bd9d-4a26-b836-d0f5c84b4773
id: ec91d09e-c5a4-447e-b5a2-fd6ee2d7f89a
---
## What is Redis?
[Redis](https://redis.io/) is a NoSQL key/value store, much like our flat file system based on [YAML](/yaml), that is held in memory. This results in very fast I/O response times and lets you scale to much larger and handle heavier traffic. It's a perfect fit for taking Statamic to the next-level.

If you're not sure if you have Redis installed, you can run the following command from the command line.

```.language-terminal
$ redis-cli
```

If you don't get an error and see something similar to this, you've got yourself some Redis.

```.language-cli
$ 127.0.0.1:6379>
```

## Installing Redis

If Redis isn't already installed on your server, or you want to get it running locally for testing, here are a few articles that can help:

- [How to Install Redis with Homebrew on OSX](https://medium.com/@djamaldg/install-use-redis-on-macos-sierra-432ab426640e)
- [How to Install Redis on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04)

## Enabling Statamic's Redis Cache driver

In your `.env` file set the following:

```.language-env
CACHE_DRIVER=redis
```

That's it. If you need to change the host, port, or database (not common, but useful if you have multiple sites on the same server that should not share a cache), you have access to the following options as well, shown with their defaults.

```.language-env
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
REDIS_DATABASE="0"
```

_Note: It's crucial that you wrap the 0 value of `REDIS_DATABASE` with quotes, otherwise it will evaluate it incorrectly, and result in an the error `\`SELECT\` failed: ERR invalid DB index BUG db is db0`._

You can also set a prefix by adding:

```.language-env
CACHE_KEY_PREFIX=my_awesome_site
```

This is vital if you are running more than one site on a server.

## How to confirm Redis is actually working

Run the `monitor` command and visit a page of the site in your browser. If it's working you'll see a number of events trigger as Redis does its thing.

```.language-cli
$ redis-cli monitor
```
