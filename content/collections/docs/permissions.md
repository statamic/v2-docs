---
title: Roles and Permissions
overview: >
  Users are any members of your site that have an account. What they can _do_ with that account is up
  to you. You can give them access to login-only areas of the front-end, give them the ability to
  manage every aspect of the site in the Control Panel, and everything in between.
id: 0b67dd06-c403-45a0-9833-ad1e62ce4ef6
---
A User by itself has no permission to access or change any aspect of Statamic except a [front-end login][front_end_login]. Beyond that, it takes an explicit set of Permissions for that User to accomplish anything.

Access to Statamic's Control Panel and the ability to access, create, edit, and delete content and settings is broken out across many different Permissions. Let's cover some lingo before we move on further.

## Super Users {#super-users}

You may have heard the term _super user_, _super admin_, or _root user_ thrown around the internet. This is a way of saying someone that has unlimited permissions.

Whenever Statamic needs to ask the question "Can this user do this thing?", if the user has _super permissions_ the answer will always be "Yes".

It is not a permission to be taken lightly, and is often only held by the developers of the site.

A user can be considered _super_ if they belong to a role or user group with a `super` permission, or if they have `super: true` directly on their account.

Super users are the only ones that are able to _configure_ the site through the Control Panel. They can create and delete content collections (entry collections, taxonomies, global sets, asset containers, etc); manage fieldsets, edit system settings, and more.

## Roles {#roles}

Assigning permissions one at a time per-user would be a tedious endeavor, much like cleaning up a bucket of Lego bricks dumped out on your kitchen floor with one hand.

For that very reason, Roles exist. A Role is simply a reusable group of Permissions that can be assigned to users and groups. For example, you could create an **Editor** role that had all the necessary permissions to create, edit, and delete content, but no access whatsoever to modify site settings or work with system tools.

A User can hold multiple Roles and it makes no difference whether there is a lot, a little, or no overlap between them. The Permissions are additive, which means if one Role gives you a Permission, no other Role can take it away. Keep that in mind as you design the best Roles for your workflow.

It is through the Roles interface that you even access the available Permissions. There is no limit to the number of Roles you can create. Feel free to get creative. We've heard rumors of a "Bacon Aficionado" Role out there in the wild, and if you're reading this and can give us access, we formally request an invite. We'll bring the shrimp.

![Bacon, ladies and gentlemen](/assets/img/other/bacon.jpg)

Roles are created and managed in `Configure » Users`, assuming you have necessary permissions to access it.


It so happens that when roles are created via the Control Panel (CP) they are identified under the hood in `/site/settings/users/roles.yaml` by a system-created alphanumeric ID, such as:
```
d32e14fb-08c9-44c2-aaf8-21200852bafd:
  title: Admin
  permissions:
    - super```
```
This works just fine, as such IDs are never seen by folks who interact with the site exclusively via the CP. However, if you are setting up your site by hand, you can opt to define roles yourself by editing `.../roles.yaml` and give them more human-readable names. For instance,
```
admin:
  title: Admin
  permissions:
    - super
```
Both approaches work equally well, but the later may be more convenient for hands-on programmers.

## Groups {#groups}

Just in case you have a complex publishing workflow or lots of cooks in the kitchen (metaphorically speaking of course - if you have lots of real cooks we'd like to try what you're cooking and formally request invites to this event too), assigning multiple Permissions to every User can _also_ be tedious, as are an over abundance of metaphors and subtexts.

Enter: **User Groups**. When a User registers on your site, or you create a user and invite them, you can isolate them to a specific group. Each group has its own assigned Roles, which makes the entire Role assigning process hands-off and automated.

Groups are created and managed in `Configure » Users`, again assuming you have the necessary permissions to access it.

## Conclusion {#conclusion}

To explain this any further would likely be futile. Log into the Control Panel, head to the `Configure` area, and start clicking. We're pretty sure you can take it from here.

[front_end_login]: /tags/user-login_form
