---
title: UserFactory
class: Statamic\Data\Users\UserFactory
id: f45eafd5-9d59-4df6-ba88-fd49d0267fcc
---
## Usage

Create a `UserFactory` using the API class.

```
$factory = Statamic\API\User::create();
```

## Available Methods

### with

Add an array of data to the object. "Create an object _with_ this data."

```
$factory->with(['foo' => 'bar', 'baz' => 'qux']);
```

### username

Specify the username.

```
$factory->username('ron');
```

### email

Specify the email address. This is an alias for passing the email in `with` data.

```
$factory->email('ron@swanson.com');
```
