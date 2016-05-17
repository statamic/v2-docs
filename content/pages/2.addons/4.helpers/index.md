---
title: Helpers
id: cea88298-9c37-425b-9bfb-58e5d9a8e7ef
overview: |
  From within any addon aspect, the following methods will be available to you.
---
The following methods can all be accessed within addon classes by using  `$this->method()`.

## General

### getAddonClassName
Returns the name of the addon, uncustomized by `meta.yaml`.

For example, if you call this method from within `site/addons/MyAddon/MyAddonTags.php`, it will return `MyAddon`.

### getAddonFQCN
Returns the fully qualified class name of appropriate addon aspect.

For example, if you call this method from within `site/addons/MyAddon/MyAddonTags.php`, it will return `Statamic\Addons\MyAddon\MyAddonTags`.

### getAddonName
Returns the name of the addon. If one has been specified in `meta.yaml`, it will use that. Otherwise it will use `getAddonClassName()`.

### getMeta
Returns the contents of `meta.yaml` as an array.

### getDirectory
Returns the directory this addon's file is in.

For example, if you call this method from within `site/addons/MyAddon/MyAddonTags.php`, it will return `site/addons/MyAddon`.

---

## Events

### emitEvent($event)
Emits an event, namespaced by your addon.

For example, `$this->emitEvent('hello')` would emit `MyAddon.hello`.

### eventUrl($url)
Returns an event URL with the $url appended to it.

For example, `$this->eventUrl('foo/bar')` would return `/!/MyAddon/foo/bar`.

---

## API

### api($addon)
Gets the API class of the specified addon.

---

## Config

### getConfig($keys, $default = null)
Retrieves a value from the addon's config. If you specify an array of keys, the first match found will be returned. If nothing is found, the default will be returned.

### getConfigBool($keys, $default = null)
Same as `getConfig`, but will convert the returned value to a boolean.

`no`, `false`, `0`, `''`, and `-1` will be treated as `false`. Anything else will be `true`.

### getConfigInt($keys, $default = null)
Same as `getConfig`, but will convert the returned value to an integer.

---

## Email

### email()
Returns an instance of a `Statamic\Email\Builder`, and sets the views path to the addon's `views` directory.

---

## Blink Cache

The Blink cache is the shortest cache. It only spans the length of the request.

### blink->get($key, $default = null)
Returns blink data saved under `$key`, or `$default` if it doesn't exist.

### blink->put($key, $value)
Saves a `$value` to the blink cache under a `$key`.

### blink->exists($key)
Checks if a given `$key` exists in the blink cache.

### blink->increment($key, $increment = 1)
Increments the value saved in `$key` by `$increment`. Assumes the value is numeric.

### blink->clear()
Clears the entire blink cache.

### blink->all()
Gets all the blink values.

---

## Cache

The system cache will be saved until it is manually purged.

### cache->get($key, $default = null)
Returns cached data saved under `$key`, or `$default` if it doesn't exist.

### cache->put($key, $value, $mins = null)
Saves a `$value` to the cache under a `$key` for a specified number of minutes. If no time is specified, it'll be saved forever.

### cache->exists($key)
Checks if a given `$key` exists in the cache.

---

## Cookie

Cookies are saved to the browser and can be cleared by a user at any point.

### cookie->get($key, $default = null)
Returns a cookie's data saved under `$key`, or `$default` if it doesn't exist.

### cookie->put($key, $value, $mins = null)
Saves a `$value` to the cookie named `$key` for a specified number of minutes. If no time is specified, it'll be saved for 5 years.

### cookie->forget($key)
Delete a cookie named `$key`.

### cookie->exists($key)
Checks if a cookie named `$key` exists.

---

## Storage

Storage will save files under `site/storage`. Using these helpers, data will automatically be saved into your addon's subfolder, eg. `site/storage/addons/MyAddon/$key`.

### storage->putYAML($key, $data)
Saves an array to storage under `$key`, as a `.yaml` file.

### storage->putSerialized($key, $data)
Saves an array to storage under `$key`, as a `.php` file with a serialized string.

### storage->putJSON($key, $data)
Saves an array to storage under `$key`, as a `.json` file.

### storage->get($key, $default = null)
Returns stored data saved under `$key`, or `$default` if it doesn't exist.

### storage->getYAML($key, $default = null)
Returns the array stored as YAML saved under `$key.yaml`, or `$default` if it doesn't exist.

### storage->getSerialized($key, $default = null)
Returns the array stored as a serialized string saved under `$key.php`, or `$default` if it doesn't exist.

### storage->getJSON($key, $default = null)
Returns the array stored as JSON saved under `$key.json`, or `$default` if it doesn't exist.
