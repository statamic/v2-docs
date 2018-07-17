---
title: response.created
class: response.created
id: 0bd05051-f84c-4422-b26d-98f5c7a92605
---
Fired just after the response has been created for front-end requests, and just before it gets sent.

This is a perfect time for you to modify the response by adding headers, adjusting the output, etc.

An `Illuminate\Http\Response` instance will be passed through. Simply modify it and don't return anything.

```
use Illuminate\Http\Response;

public $events = ['response.created' => 'handle'];

public function handle(Response $response);
```
