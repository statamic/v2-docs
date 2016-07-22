---
title: Role
class: Statamic\API\Role
id: 795ff9b8-7395-4547-ad72-9d4426711da9
---
## Get all roles.

``` php
Role::all(); // Returns \Illuminate\Support\Collection
```

## Get a role by ID.

``` php
Role::find($id); // Returns Role
```

## Check if a role exists.

``` php
Role::exists($id); // Returns a boolean
```

## Get a role by handle.

``` php
Role::whereHandle($handle); // Returns Role
```
