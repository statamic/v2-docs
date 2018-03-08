---
title: Caching with Redis
overview: If you find yourself in a situation with a huge site, small amount of memory, and static caching isn’t an option, using a Redis cache may be a great way to increase site performance.
kb_categories:
  - Tips, Tricks, and How-Tos
related_reading:
  - 102f8c10-9120-4a6e-a630-97050b494b89
  - 0b47d058-a8cc-41ec-9938-b9ef68c03d8d
  - e31e44d1-a82d-438d-9a8d-5a22fe4b76e3
id: ec91d09e-c5a4-447e-b5a2-fd6ee2d7f89a
---
## What is Redis?
[Redis](https://redis.io/) is a NoSQL key/value store, much like our flat file system based on [YAML](/yaml), that is held in memory with very fast IO capabilities. It's a perfect choice for Statamic sites.

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
- [How to Install Reis on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04)

## Enabling Statamic's Redis Cache driver

In your `.env` file set the following:

```.language-env
CACHE_DRIVER=redis
```

That's it. If you need to change the host, port, or database (not common), you have access to the following options as well, shown with their defaults.

```.language-env
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
REDIS_DATABASE=0
```

## How to confirm Redis is actually working

Run the `monitor` command and visit a page of the site in your browser. If it's working you'll see a number of events trigger as Redis does its thing.

```.language-cli
$ redis-cli monitor
```
