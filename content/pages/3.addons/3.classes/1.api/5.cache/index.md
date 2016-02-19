---
title: Cache
class: 'Statamic\API\Cache'
overview: "A convenience wrapper around Laravel's Cache methods."
methods:
  -
    name: put
    description: |
      Save a key to the cache. You may optionally specify the length (in minutes) to keep this in the cache. Otherwise, it will stay there until manually cleared.

      ```
      Cache::put('foo', $foo, 10);
      ```
    signature: $key, $data, $mins = null
  -
    name: get
    description: |
      Get a `$key` from the cache. If it's not there, `$default` will be returned.

      ```
      Cache::get('hello', 'world);
      ```
    signature: $key, $default = null
id: e5e5facc-03cf-43a6-a94b-997209a86aed
---
## Addon Best Practice

These class methods save directly to the cache - without any namespacing. You should be careful about collisions with other addons.

It's a better idea to use the `$this->cache()` addon helper, as it will auto-namespace your keys.
