---
class: Statamic\Data\Users\User
title: User
id: 57156605-8761-42f3-8a7f-97b61cd70441
---
Inheritance: [Data](/addons/api/data)

## username

Get or set the username.

```
$user->username(); // Returns a string

$user->username('bob'); // Returns null
```

## email

Get or set the email.

```
$user->email(); // Returns a string

$user->email('bob@down.com'); // Returns null
```

## password

Get or set the password.

Getting the password will first ensure that it is secured, and return the hashed version.

```
$user->password(); // Returns a string
```

Setting the password will automatically secure it.

```
$user->password('foo'); // Returns null
```

## isSecured

Check whether the password has been secured (hashed).

```
$user->isSecured(); // Returns a boolean
```

## ensureSecured

Ensures that the password is secured (hashed).

If they don't have a password, there's nothing to secure, so nothing will happen.

The first parameter is a boolean denoting whether or not to save the user after securing. By default it _will_ be saved.

```
$user->ensureSecured(); // Returns null
```

## securePassword

Secure the password. This will make sure that the plain text password is replaced by a hashed version.

The first parameter is a boolean denoting whether or not to save the user after securing. By default it _will_ be saved.

```
$user->securePassword(); // Returns null
```

## status

Get the status of the user.

A user is `pending` if they don't yet have a password. Otherwise, they are `active`.

```
$user->status(); // Returns a string, eg. 'pending' or 'active'
```

## can

Determine if the user can perform a given ability.

```
$user->can('cp:access'); // Returns a boolean
```

## cant

The inverse of `can`.

```
$user->cant('cp:access'); // Returns a boolean
```

## cannot

An alias of `cant`.

```
$user->cannot('cp:access'); // Returns a boolean
```

## roles

Get the roles for the user.

```
$user->roles(); // Returns \Illuminate\Support\Collection
```

## hasRole

Check if the user has a given role. You can pass an instance of `Role` or an ID.

```
$user->hasRole($role); // Returns a boolean
```

## hasPermission

Check if the user has a given permission.

```
$user->hasPermission('cp:access'); // Returns a boolean
```

## permissions

Get all a user's permissions.

```
$user->permissions(); // Returns an array
```

## isSuper

Check if the user is 'super'.

```
$user->isSuper(); // Returns a boolean
```

## groups

Get all the groups a user belongs to.

```
$user->groups(); // Returns \Illuminate\Support\Collection
```

## inGroup

Check if the user is in a given group. You can pass either a `UserGroup` or an ID.

```
$user->inGroup($group); // Returns a boolean
```

## getAuthIdentifier

Used by Laravel's authentication to find the identifier. An alias of `$user->id();`.

```
$user->getAuthIdentifier(); // Returns a string
```

## getAuthPassword

Used by Laravel's authentication to find the user's password. An alias of `$user->password();`.

```
$user->getAuthPassword(); // Returns a string
```

## getRememberToken

Used by Laravel's `Authenticatable` contract to get the "remember me" token.

```
$user->getRememberToken(); // Returns a string
```

## setRememberToken

Used by Laravel's `Authenticatable` contract to set the "remember me" token.

```
$user->setRememberToken($token); // Returns null
```

## getRememberTokenName

Used by Laravel's `Authenticatable` contract to get the "remember me" token me name.

```
$user->getRememberTokenName(); // Returns 'remember_me'
```

## setPasswordResetToken

Used by Laravel's `Authenticatable` contract to set the password reset token.

```
$user->setPasswordResetToken($token); // Returns null
```

## getPasswordResetToken

Used by Laravel's `Authenticatable` contract to get the password reset token.

```
$user->setPasswordResetToken($token); // Returns null
```

## getOAuthId

Get the user's OAuth ID for a given provider.

```
$user->getOAuthId($provider); // Returns a string
```

## setOAuthId

Set the user's OAuth ID for a given provider.

```
$user->setOAuthId($provider, $id); // Returns null
```
