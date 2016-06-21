---
title: "The Roles, Permissions, & Groups That Bind Us"
nav_title: Roles & Permissions
cover: /assets/img/trail-guides/permissions.jpg
description: >
  Manage users, customize permissions, create roles, and organize them into happy little groups.
overview: >
  Users come in a number of shapes and sizes. They may or may not have an account; you may want to give them
  the keys to the castle, or throw them into the fires of Mount Doom. What they can and can't do is up to you.

id: cd28440d-46a8-4de7-b91a-c8eb40d14bfa
---
Users and Permissions is a broad subject, so we'll start off explaining [what permissions are](#permissions), and how to configure them. Then we'll dive into how you can [protect your content](#protecting-content) from users - which may mean users with and without accounts. Onwards!

# Permissions {#permissions}

A User by itself has no permission to access or change any aspect of Statamic except a [front-end login][front_end_login]. Beyond that, it takes an explicit set of Permissions for that User to accomplish anything.

Access to Statamic's Control Panel and the ability to access, create, edit, and delete content and settings is broken out across many different Permissions. Let's cover some lingo before we move on further.

## Super Users

You may have heard the term _super user_, _super admin_, or _root user_ thrown around the internet. This is a way of saying someone that has unlimited permissions.

Whenever Statamic needs to ask the question "Can this user do this thing?", if the user has _super permissions_ the answer will always be "Yes".

It is not a permission to be taken lightly, and is often only held by the developers of the site.

A user can be considered _super_ if they belong to a role or user group with a `super` permission, or if they have `super: true` directly on their account.

Super users are the only ones that are able to _configure_ the site through the Control Panel. They can create and delete content collections (entry collections, taxonomies, global sets, asset containers, etc); manage fieldsets, edit system settings, and more.

## Roles

Assigning permissions one at a time per-user would be a tedious endeavor, much like cleaning up a bucket of Lego bricks dumped out on your kitchen floor with one hand.

For that very reason, Roles exist. A Role is simply a reusable group of Permissions that can be assigned to users and groups. For example, you could create an **Editor** role that had all the necessary permissions to create, edit, and delete content, but no access whatsoever to modify site settings or work with system tools.

A User can hold multiple Roles and it makes no difference whether there is a lot, a little, or no overlap between them. The Permissions are additive, which means if one Role gives you a Permission, no other Role can take it away. Keep that in mind as you design the best Roles for your workflow.

It is through the Roles interface that you even access the available Permissions. There is no limit to the number of Roles you can create. Feel free to get creative. We've heard rumors of a "Bacon Aficionado" Role out there in the wild, and if you're reading this and can give us access, we formally request an invite. We'll bring the shrimp.

![Bacon, ladies and gentlemen](/assets/img/other/bacon.jpg)

Roles are created and managed in `Configure » Users`, assuming you have necessary permissions to access it.

## Groups

Just in case you have a complex publishing workflow or lots of cooks in the kitchen (metaphorically speaking of course - if you have lots of real cooks we'd like to try what you're cooking and formally request invites to this event too), assigning multiple Permissions to every User can _also_ be tedious, as are an over abundance of metaphors and subtexts.

Enter: **User Groups**. When a User registers on your site, or you create a user and invite them, you can isolate them to a specific group. Each group has its own assigned Roles, which makes the entire Role assigning process hands-off and automated.

Groups are created and managed in `Configure » Users`, again assuming you have the necessary permissions to access it.

## Conclusion

To explain this any further would likely be futile. Log into the Control Panel, head to the `Configure` area, and start clicking. We're pretty sure you can take it from here.

[front_end_login]: /reference/tags/user-login_form


# Protecting Content {#protecting-content}

You may protect your content from unwanted visitors with the `protect` variable. By setting this variable you can
allow or deny people in various ways. There are a number of _schemes_ used for protecting content:

- [Password Protection](#password-protection) will force users to enter a password you set before being allowed to view the content.
- [IP address](#ip-address-protection) whitelisting.

The `protect` variable can be placed in a number of places:

- In _content_ (pages, entries, and taxonomy terms.)
- In `folder.yaml` files, which would cascade down and protect all entries, terms, child pages.
- Directly into routes.

To protect content, you should add a `protect` array, like so:

``` .language-yaml
title: My Protected Content
protect:
  password:
    allowed: ['secret']
```

This example uses the [password scheme](#password-protection), but you should simply place whatever scheme
you require in there. They're all documented below.

Whichever scheme choose, know that `protect` is designed to help you out. We’ve tried to keep the syntax as simple as possible to use while at the same time allowing for a lot of flexibility. Because of this, if Statamic sees that `protect` has been set but it isn’t able to parse the requirements you’ve set, _all_ users will _always_ be denied. In these instances, a message will be written to the log telling you that something’s gone wrong.

## Configuring {#configuring-protection}

There are some aspects to content protection that you may like to apply across your site. The following settings
can be added to `site/settings/system.yaml`:

- `protect_password_form_url` is where a user will be redirected when a `password` scheme is set but the user has not yet entered in a valid password.

## Password protection {#password-protection}

There will be times when you want to password-protect one or more files, but don’t want to bother with having people create member accounts just to access a page. That is where using the `password` scheme comes in. This scheme does not relate to user accounts in any way, only one-off password entry. The setup for this would look something like the following:

``` .language-yaml
protect:
  password:
    allowed: [ "my-password", "another-password" ]
    form_url: /password-entry
```

This tells Statamic to `protect` this content file with a password. You provide one or more valid allowed passwords (case-sensitive), and that it should forward people that haven’t entered the correct password to `form_url` (either another page or route you've set up).

Note: the `form_url` variable is optional here, if you don’t include it, `protect` will use the URL configured in  `protect_password_form_url` in `system.yaml`

### Password Form

Next, you’ll need to provide a way for people to enter passwords for URLs. In the above example, `form_url` is pointing to `/password-entry`, but this can be anywhere on your site. The easiest way is to point a route at a template:

``` .language-yaml
routes:
  /password-entry: password-entry
```

```
{{ protect:password_form }}
    {{ if no_token }}
        <p>No token has been provided.</p>
    {{ else }}
        {{ if error }}
            <p class="error">{{ error }}</p>
        {{ /if }}

        Password:
        <input type="password" name="password" />

        <button type="submit">Submit</button>
    {{ /if }}
{{ /protect:password_form }}
```

The `protect:password_form` tag is going to wrap everything between the tags in an HTML form tag that’s pointing to the appropriate place. Note that the HTML of the form itself is up to you. The only requirements of this form are that the user is entering passwords into a field named `password`. Other than that you can do anything you’d like.

A token will be added to the URL when redirected from the protected page. If you visit the password page without a token, `no_token` will be true, and you can output a message if you like.

### Invalid Passwords

If someone submits a password and it isn’t valid, Statamic will redirect the user back to the form, populating the error tag with an error message. In the example above, you can see that we’re printing it out if it’s there. Valid passwords can vary from piece of content to piece of content. This one form is smart enough to handle all password management between password-protected URLs.

### Valid Passwords

A valid password is one that matches any of the passwords in the allowed list as configured on the page. This means that you can send three people three different passwords to access the same file, each having their own way in. Additionally, you could also set just one password and send that to 100 people and they can all use the same password.

It should go without saying, but for the sake of completeness here be careful in how you set and give out passwords.

### Password Expiration

Each user’s passwords will expire once their session has expired. To manually invalidate a password, simply remove it from the list of allowed passwords on the page. The next time a user with that password visits this page, they’ll be redirected to the password form just like everyone else.


## IP Address Protection {#ip-address-protection}

This scheme is super simple in that people will either be allowed in or not. The setup looks like this, similar to `password`:

``` .language-yaml
protect:
  ip_address:
    allowed: [ 127.0.0.1 ]
```

We set the scheme to be `ip_address`, and create a list of `allowed` IP addresses. If the visitor’s IP address matches one of the IP addresses in this list, they will be allowed in, otherwise, they will be shown the access denied template.

## Site-wide Protection {#sitewide-protection}

Instead of just applying protection to content one by one, it's possible to protect your whole site. This could be useful if you'd like show the site to your client but don't want anyone else to see, for example. You can even pair this with [environment specific settings](/knowledge-base/settings) for added flexibility.

To do this, simply add a `protect` scheme to your `site/settings/system.yaml` file.
