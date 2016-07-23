---
title: UserGroup
class: Statamic\API\UserGroup
id: bee2a2d5-25c5-42a9-954c-c5597c7437fa
---
## Get all groups.

``` php
UserGroup::all(); // Returns \Illuminate\Support\Collection
```

## Get a group by ID.

``` php
UserGroup::find($id); // Returns UserGroup
```

## Check if a group exists.

``` php
UserGroup::exists($id); // Returns a boolean
```

## Get a group by handle.

``` php
UserGroup::whereHandle($handle); // Returns UserGroup
```

## Get a user's groups.

``` php
UserGroup::whereUser($user); // Returns \Illuminate\Support\Collection
```

This is the same getting the groups from the user, and depending on your use case, you might prefer doing this:

``` php
$user->groups();
```
