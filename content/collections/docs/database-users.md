---
title: Database Users
overview: |
  Store your users in a database.
id: 207322b1-fdb9-44be-a181-93f1dc5893fb
---
## Overview

If you have a large amount of users, or you need to scale horizontally, it may make sense to store them in a database.

You may choose to store users in a traditional database using the [Eloquent](#eloquent) driver, or piggybacking on the Stache using the [Redis](#redis) driver.

## Eloquent {#eloquent}

### Installation {#eloquent-installation}

Enable the driver in your `site/settings/users.yaml` file:

``` .language-yaml
driver: eloquent
```

In your `.env` file, add the following variables and customize as necessary:

```
DB_HOST=localhost
DB_DATABASE=database-name
DB_USERNAME=username
DB_PASSWORD=password
```

Generate the migration file into `site/database/migrations` by running the following command:

```
php please make:user-migration
```

Modify the migration file as necessary:

  - You may [optionally](#eloquent-oauth) separate the `name` column into `first_name` and `last_name` columns.
  - If you don't plan to use OAuth, you can remove the references in the `up`/`down` methods.

Modify your `site/settings/fieldsets/user.yaml` fieldset as necessary:

  - Ensure you have the correct name `name` (or `first_name` and `last_name`) field(s) to match your migration.
  - Remove the `content` biography field, unless you've specified a matching column in your migration.
  - etc.

Make sure the database you specified in your `.env` file exists and run the migration:

```
php please migrate
```

### OAuth {#eloquent-oauth}

If you chose to separate the `name` column into `first_name` and `last_name`, you should be aware that the default OAuth implementation will try to save a `name` value to the user and result in an exception.

You will need to customize the [user data creation](/oauth#user-data-creation) to handle the two columns.

## Redis {#redis}

The Redis user driver assumes you are using Redis as the cache driver.

### Installation {#redis-installation}

Ensure you are using Redis as the cache driver:

```
CACHE_DRIVER=redis
```

Enable the driver in your `site/settings/users.yaml` file:

``` .language-yaml
driver: redis
```

### Persistence {#redis-persistence}

By default, user files will still be saved to disk. This will allow you to track them in source control.
Additionally, when your Redis cache is empty, any user files will be read from disk.

If you don't have a need to track user files, you may disable them being written to disk inside `site/settings/users.yaml`:

``` .language-yaml
redis_write_file: false
```

A typical example where you may not care about keeping user files is if you are using OAuth. The users will be re-created
on the fly whenever they re-authenticate with the provider.
