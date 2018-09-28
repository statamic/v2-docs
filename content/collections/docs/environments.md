---
title: Environments
id: 0b47d058-a8cc-41ec-9938-b9ef68c03d8d
overview: >
  It is often beneficial to have different settings depending on where you are running the site. For instance, you might want to enable debug mode when in development, but not in production.
---

## The .env file {#the-env-file}

Environment variables are contained in a file named `.env`, kept in the root of your site. Many system settings can be affected here, and you can also set your own variables to use in any of Statamic's system settings.

The `.env` syntax is a key/value pair that looks exactly like this:

```
MY_VARIABLE=value
DELICIOUSNESS=bacon
```

You should _not_ commit your `.env` file to version control. It may contain confidential information (such as API keys), and each developer and/or server may require a different configuration setting.

If you are working on a team, you may wish to continue including a `env.example` file filled with placeholder values. This is common best practice, allowing other developers to get up and running quickly.

## Defining Environments {#defining-environments}

Statamic runs in the `production` environment by default, unless otherwise specified. To specify a another environment, add `APP_ENV=foo` to your `.env` file, where `foo` is the name of your environment.

## Environment Settings {#environment-settings}

There are two options when it comes to environment-specific settings. You can mix-and-match them, using whichever approach feels most logical to you.

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

When interpolating, make sure to wrap your value in quotes.

> Some of the `env:` code examples on this page use an invisible character to prevent it from actually rendering the environment variable. If you copy/paste directly from here, be sure to delete it - or just write it by hand!

### Option 2: Environment files {#option-environment-files}

The second option is to leverage an environment system settings file, located in `site/settings/environments/`. Each `yaml` file here becomes an environment name, and any data defined here is added to the [cascade](/cascade) when running in that specific environment.

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

Anything nested under `settings` would override values in the corresponding yaml file. In this example, `debug` and `debug_bar` will override the values in `debug.yaml`.

Anything nested under `addons` would override values in the corresponding addon's yaml file. In this example, `slices` will override the value in the `bacon` addon's setting file.

You may also interpolate in your environment files, if you wish.

``` .language-yaml
settings:
  debug:
    debug: "{env:APP_DEBUG}"
```

## Editing environment settings {#editing-environment-settings}

When an environment setting has been used, it becomes uneditable through the control panel. This provides clarity in the UI as to where the value is coming from.
