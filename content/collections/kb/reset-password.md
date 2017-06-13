---
title: Resetting Your Password
id: 53965b39-0e0c-4435-8b1e-b667bf09cb30
kb_categories:
  - Tips, Tricks, and How-Tos
---
Forgot your admin user's password? No problem.

Open `site/users/<your_user>.yaml`, remove `password_hash`, and set a new password like so:

```
password: supersecretpassword!
```

Once you login it'll be encrypted and you'll be on your way, safe and sound.
