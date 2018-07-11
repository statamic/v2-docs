---
title: glide.generated
class: glide.generated
id: 5ef3b6c0-323b-4276-b478-6a95dd74f537
---
Fired after Glide has generated an image. The parameters will be `$path` - which will be the full path to the cached image, and `$params` - an array of query parameters used to generate the image.

```
public $events = ['glide.generated' => 'handle'];

public function handle($path, $params);
```
