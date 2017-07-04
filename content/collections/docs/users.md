---
title: Users
id: d744ff59-3507-419d-9808-84079d8111a4
overview: >
  Normally a User is someone with an account and access to the [Control Panel](/control-panel), but that's not _always_ the case. Statamic's flexible User system gives you a lot of flexibility around what being a "User" can mean.
---

## Creating Your First User {#creating-your-first-user}

Your first User account is normally created during [installation][installation], but you can also create one by hand.

Users are represented by YAML files inside `site/users`. The filename becomes the username, and you can hold any "profile" data you'd like inside, as valid YAML. Here's an example.

```.language-yaml
first_name: Joe
last_name: Cool
email: snoopy@blockhead.com
super: true
password: YourPasswordGoesHere
```

Two important things to note here: the `super`, and the `password`.

### The Password {#the-password}

Creating a user by hand will involve a plain text password. Have no fear, however! As soon as you log in, it will be encrypted and hashed and as secure as any DB driven auth system you've ever seen (you can log in by visiting `/cp`).

### Custom Profile Fields {#custom-profile-fields}

Want a more robust profile for your users that you can manage with the Control Panel? Need to add a phone number, current address, favorite character from The Office?

Create a new fieldset at `/site/settings/fieldsets/user.yaml` and add all the fields you need. Remember to add in all the default fields or they will disappear in the Control Panel for all your users. Here's an example.

```.language-yaml
hide: true
fields:
  username:
    type: text
    display: Username
    width: 50
  roles:
    type: user_roles
    width: 50
  email:
    type: text
    display: Email address
  first_name:
    type: text
    display: First Name
    width: 50
  last_name:
    type: text
    display: Last Name
    width: 50
  phone-number:
    type: text
    display: Phone Number
    width: 50
  content:
    type: textarea
    display: Biography
```

## Permissions {#permissions}

Statamic has a whole [Permissions and Roles][permission] system letting you create and configure some pretty granular controls over who can see and do what in the Control Panel.

You can bypass all that by setting `super: true`. You'll have access to everything. And as the developer of a new Statamic site, that's probably what you want.

Users without a role, or `super` status will not have access to the Control Panel at all. You can use that state to create member areas on the front-end of your site. We have a whole bunch of Tags to facilite that workflow.

[installation]: /installing
[permission]: /permissions
