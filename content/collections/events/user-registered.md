---
title: user.registered
class: user.registered
id: c59712b5-7299-4ea9-b318-84877960a0ab
---
Fired after a user has registered with the [Register Form](/tags/user-register_form). The `User` will be passed through.

```
public $events = ['user.registered' => 'handle'];

public function handle(User $user);
```
