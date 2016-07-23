---
title: User
class: Statamic\API\User
id: fbf0cbb3-7019-4f16-b0ea-f3447aeebf12
---
## Get all users.

``` php
User::all(); // Returns UserCollection
```

## Get a user by ID.

``` php
User::find($id); // Returns User
```

## Get a user by username.

``` php
User::whereUsername($username); // Returns User
```

## Get a user by email.

``` php
User::whereEmail($email); // Returns User
```

## Get a user by their OAuth credentials.

``` php
User::whereOAuth($provider, $id); // Returns User
```

## Get the currently authenticated user.

``` php
User::getCurrent(); // Returns User if logged in
```

## Check whether the user is logged in.

``` php
User::loggedIn(); // Returns a boolean
```

## Create a user.

This returns an instance of a `UserFactory` to allow you to chain and build your user.

```
User::create(); // Returns UserFactory
```
