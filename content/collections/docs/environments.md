---
title: Environments
id: 0b47d058-a8cc-41ec-9938-b9ef68c03d8d
overview: >
  It is often beneficial to have different settings depending on where you are running the site. For instance, you might want to enabele debug mode when in development, but not in production.
---

## The .env file {#the-env-file}

Environment variables are contained in a file named `.env`, kept in the root of your site. Many system settings can be affected here, and you can also set your own variables to use in any of Statamic's system settings.

The `.env` syntax is a key/value pair that looks exactly like this:

```
MY_VARIABLE=value
DELICIOUSNESS=bacon
```

You should _not_ commit your `.env` file to version control. It may contain confidential information (such as API keys), and each developer and/or server may require a different configuration setting.

If you are working on a team, you may wish to continue including a `env.example` file filled with placeholder values. This is common best practice, alloweding other developers to get up and running quickly.

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

_Note: When interpolating, make sure to wrap your value in quotes._

### Option 2: Environment files {#option-environment-files}

The second option is to leverage an environment system settings file, located in `site/settings/environments/`. Each `yaml` file here becomes an environment name, and any data defined here is added to the [cascade][/cascade] when running in that specific environment.

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

Nesting must always be from the "root" (`settings:`) and include any pre-existing nesting already within whichever `.yaml` file for the value you wish to override. For instance, if you want to override just the `url:` field in `settings.yaml`, 

```
app_key: …
license_key: …
locales:
  en:
    full: en_US
    name: English
    url: http://example.dev
```

you'd need to include a full, properly indented path in your named environment's `.yaml` file, called e.g., `ON-REMOTE-SERVER.yaml`:

```
settings:
  system:
    locales:
      en:
        url: http://example.com
```

Pair that with a suitable `.env` file on the server (only),

```
APP_ENV=ON-REMOTE-SERVER
```

so that whenever you upload your work to that server, the `site_url` global variable will "automatically" take on a `.com` extention in place of `.dev`. (Note the `.dev` default in `settings.yaml` already has your development environment covered, so there's perhaps no need for a parallel local environment file (e.g., `ON-MY-LAPTOP.yaml`) unless you're locally overriding other settings too.)

You may also interpolate in your environment files, if you wish.

``` .language-yaml
settings:
  debug:
    debug: "{env:APP_DEBUG}"
```

## Editing environment settings {#editing-environment-settings}

When an environment setting has been used, it becomes uneditable through the control panel. This provides clarity in the UI as to where the value is coming from.
