---
title: Environments
id: 0b47d058-a8cc-41ec-9938-b9ef68c03d8d
overview: >
  Sometimes it's beneficial to have different settings depending on where you are running the site. For instance, enabling debug mode when in development, but not in production.
---

## The .env file {#the-env-file}

The `.env` contains environment level variables. These can be used within Statamic settings files and are also used to change Statamic's behavior.

The syntax in this file is:

```
MY_VARIABLE=value
DELICIOUSNESS=bacon
```

Your `.env` file should _not_ be committed to version control, since it may contain confidential information, and since each developer / server could require a different environment configuration.

If you are developing with a team, you may wish to continue including a `env.example` file. By putting placeholder values in the example file, other developers on your team can clearly see which environment variables are needed to run the site.

## Defining Environments {#defining-environments}

By default, Statamic runs in the `production` environment.

To specify a different environment, add `APP_ENV=foo` to your `.env` file, where `foo` is the name of your environment.

## Environment settings {#environment-settings}

There are two options when it comes to environment-specific settings. You can mix-and-match them. Whatever feels natural to you.

### Option 1: Interpolation {#option-interpolation}

The first option is to add the `.env`-based variables directly into settings files using the `{env‌:}` helper.

For example, in `debug.yaml`, you could do this:

``` .language-yaml
debug: "{env‌:APP_DEBUG}"
```

and set `APP_DEBUG=true` in your `.env` file.

You may use the variable in the middle of a string, for example:

``` .language-yaml
greeting: "Hello {env:NAME}"
```

```
NAME=ron
```

Note: When interpolating, make sure to wrap your value in quotes.

### Option 2: Environment files {#option-environment-files}

The second option is to leverage the environment settings file.

Statamic will load `site/settings/environments/foo.yaml` where `foo` is the current environment.

In here, any variables are added to the cascade.

For instance:

``` .language-yaml
settings:
  debug:
    debug: true
    debug_bar: true
addons:
  bacon:
    slices: 20
```

Anything nested under `settings` would override values in the corresponding yaml file. So in this example, `debug` and `debug_bar` will override the values in `debug.yaml`.

Anything nested under `addons` would override values in the corresponding addon's yaml file. So in this example, `slices` would override the value in the `bacon` addon's setting file.

You can also interpolate in your environment files, if you wish.

``` .language-yaml
settings:
  debug:
    debug: "{env:APP_DEBUG}"
```

## Editing environment settings {#editing-environment-settings}

When an environment setting has been used, it becomes uneditable through the control panel. This prevents confusion on where the value is coming from.
