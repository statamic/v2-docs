---
title: Protecting Content
overview: Control who can or cant see each page of content. Or, even protect your whole site in one fell swoop.
id: 87f7d503-1bbb-4cd4-95b6-073724739339
---

## Overview {#overview}

As of 2.6, you may protect your content from unwanted visitors with the `protect` variable. By setting this variable, you can allow or deny people based on their logged-in status, IP address, passwords, and more.

There are a number of _schemes_ used for protecting content:

- [`ip_address`](#ip_address) for allowing specific IP addresses.
- [`logged_in`](#logged_in) for only allowing authenticated users.
- [`password`](#password) will force users to enter a specified password.

Whichever of these you choose, know that `protect` is designed to help you out. We've tried to keep the syntax as simple as possible while allowing for flexibility. Because of this, if Statamic sees that you've set `protect`, but it's invalid, _all_ users will _always_ be denied. 


## Basic Usage {#usage}

To protect a page, add a `protect` array to the front-matter, with the corresponding scheme's configuration inside it.

For example, to use the [password](#password) scheme, you might do something like this:

``` .lang-yaml
---
title: Top Secret
protect:
  type: password
  allowed: ['secret']
  form_url: /password-entry
---
Some top secret content.
```

You may protect an entire folder of pages by adding the `protect` variable to a `folder.yaml`.

> If you are familiar with this feature from v1, take note that the syntax has been simplified.

## Configuration {#configuration}

If you are protecting multiple pages, you can set common values for each schema type into your settings, which will cascade down to each page usage. You may add these to the `protect` variable inside your `site/settings/system.yaml` file.

Take the following settings for example:

``` .lang-yaml
protect:
  password:
    form_url: /password-entry
  logged_in:
    login_url: /login
```

Since the `password`'s `form_url` has been set here, you won't need to define it on any pages that should be password protected.

``` .lang-yaml
---
title: Top Secret
protect:
  type: password
  allowed: ['secret']
  # form_url is not necessary now, since it'll cascade from your configuration.
---
Some top secret content.
```


### Shorthand Usage {#shorthand}

If you'd like to protect a page, and have provided enough configuration for that scheme in your settings file, you may simply reference the scheme name as a string:

``` .lang-yaml
title: My Secret Page
protect: password
```


## IP Address Protection {#ip_address}

Add the IP address(es) you wish to allow to the aptly named `allowed` array.

``` .lang-yaml
protect:
  type: ip_address
  allowed:
    - 127.0.0.1
    - 192.168.0.10
```


## Logged In Protection {#logged_in}

Adding this scheme to a page will redirect to a login page unless the user is already logged in.

``` .lang-yaml
protect:
  type: logged_in
  login_url: /login
  append_redirect: true
```

If the `login_url` has not been defined, the user will see an "Access Denied" page instead of a login screen. In this case, the user could log in through the Control Panel.

The `append_redirect` setting will add `?redirect=/the-protected-url` to your `login_url`. This pairs with the [user:login_form tag's allow_request_redirect parameter](/tags/user-login_form#parameters) which will redirect the user to the intended page once logging in.


## Password Protection {#password}

There will be times when you want to password-protect one or more files, but don’t want to bother with having people create member accounts just to access a page. That is where using the password scheme comes in. This scheme does not relate to member accounts in any way, only one-off password entry. 

The setup for this would look something like the following:

``` .lang-yaml
---
title: My Secret Content
protect:
  type: password
  allowed:
    - secret
    - confidential
  form_url: /password-entry
---
You can only see me if you have the password.
And since you're reading this, you obviously do.
```


### The password form {#password-form}

Next, you’ll need to provide a way for people to enter passwords for URLs. In the above example, `form_url` is pointing to `/password-entry`, but this can be anywhere on your site.

You can either create a page, or a [route](/routing) for this. In the corresponding template, you should create something like this:

```
{{ protect:password_form }}

    {{ if no_token }}
        No token has been provided.
    {{ /if }}

    {{ if error }}
        {{ error }}
    {{ /if }}

    <input type="password" name="password" />

    <button>Submit</button>

{{ /protect:password_form }}
```

The `protect:password_form` tag is going to wrap everything between the tags in an HTML form tag that's pointing to the appropriate place. Note that the HTML of the form itself is up to you. The only requirements of this form are that the user is entering passwords into a field named `password`. Other than that you can do anything you'd like.

### Token {#password-token}

When visiting a password protected page, Statamic will generate a token and append it to the form's URL. Without a token, the form cannot function correctly. In the example above, you can see that the `no_token` boolean will be populated for you. This may happen if you visit the form URL directly.

### Invalid Passwords {#invalid-passwords}

If someone submits a password and it isn’t valid, Statamic will redirect the user back to the form, populating the `error` tag with an error message. In the example above, you can see that we’re printing it out if it’s there. Valid passwords can vary from piece of content to piece of content. This one form is smart enough to handle all password management between password-protected URLs.

### Valid Passwords {#valid-passwords}

A valid password is one that matches any of the passwords in the `allowed` list as configured on the page (or sitewide). This means that you can send three people three different passwords to access the same URL, each having their own way in. Additionally, you could also set just one password and send that to 100 people and they can all use the same password.

It should go without saying, but for the sake of completeness here be careful in how you set and give out passwords.

### Password Expiration {#password-expiration}

Each user’s passwords will expire once their session has expired. To manually invalidate a password, simply remove it from the list of allowed passwords on the page. The next time a user with that password visits this page, they’ll be redirected to the password form just like everyone else.


## Complete "Whole Hog" Protection {#complete}

If you want to protect a page from anyone - regardless of authentication status, IP address, time of day, weather, or beverage preference - you can simply add `protect: true` to the front-matter.

One may find this useful to quickly disable something.



## Site-wide Protection {#site}

To protect your whole site at once, you can simply add `type: [scheme name]` into the `protect` [configuration](#configuration) variable mentioned above.

For example, to make sure your whole site is only accessible to a single IP address, you could add `type: ip_address` to your `system.yaml` like so:

``` .lang-yaml
protect:
  type: ip_address
  ip_address:
    allowed: [127.0.0.1]
```

> This feature affects only the front-end of your site. Site-wide protection will not apply to the Control Panel.
