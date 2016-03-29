---
title: >
  The Roles, Permissions, & Groups That
  Bind Us
cover: /assets/img/trail-guides/permissions.jpg
description: >
  Manage users, customize permissions, create roles, and organize them into happy little groups.
overview: >
  Users are any members of your site that have an account. What they can _do_ with that account is up to you. You can give them access to login-only areas of the front-end, give them the ability to manage every aspect of the site in the Control Panel, and everything in between.
state: complete

id: cd28440d-46a8-4de7-b91a-c8eb40d14bfa
---
A User by itself has no permission to access or change any aspect of Statamic except a [front-end login][front_end_login]. Beyond that, it takes an explicit set of Permissions for that User to accomplish anything.

Access to Statamic's Control Panel and the ability to access, create, edit, and delete content and settings is broken out across more than 60 different Permissions. Let's cover some lingo before we move on further.

# Permission Aspects

## Admin

The **admin** permission gives a user full and complete access to everything protected by Permissions. It is not a permission to be taken lightly, and is often only held by the developers of the site.

## Access

**Access** gives a user the ability to simply get to and see the contents of an aspect of the site. The permission `cp.access` grants a user access to the Control Panel.

## Manage

The ability to **manage** an aspect of the Control Panel means that a user will be able to access, create, edit, and publish or save that type of content, or setting. For example, a user with the Permission to **manage** the Blog collection will be able to create new blog entries as well as edit and publish changes to existing blog entries.

## Delete

The permission to **delete** an aspect of the Control Panel is separate from **manage** as it has greater implications to the site as a whole. Deleting pages can reorganize navigation, create `404` errors with any hard-coded links, and so on. For these reasons, the **delete** permission is separate.

## Implicit Permissions

It's important to understand the difference between implicit and explicit permissions. When viewing the list of available permissions, you may see that you can restrict control at the Content Type level and also at the individual item level. In other words, a user can have the ability to manage Collections, or an isolated ability to manage a single collection, such as a Blog. The permission to manage Blog is an explicit one, in that it was directly assigned. The permission to manage Collections, on the other hand, is implicit, as it grants the ability to manage all Collections, existing _and_ future.

# Roles

Assigning permissions one at a time per-user would be a tedious endeavor, much like cleaning up a bucket of Lego bricks dumped out on your kitchen floor with one hand.

For that very reason, Roles exist. A Role is simply a reusable group of Permissions that can be assigned to users and groups. For example, you could create an **Editor** role that had all the necessary permissions to create, edit, and delete content, but no access whatsoever to modify site settings or work with system tools.

A User can hold multiple Roles and it makes no difference whether there is a lot, a little, or no overlap between them. The Permissions are additive, which means if one Role gives you a Permission, no other Role can take it away. Keep that in mind as you design the best Roles for your workflow.

It is through the Roles interface that you even access the available Permissions. There is no limit to the number of Roles you can create. Feel free to get creative. We've heard rumors of a "Bacon Aficionado" Role out there in the wild, and if you're reading this and can give us access, we formally request an invite. We'll bring the shrimp.

![Bacon, ladies and gentlemen](/assets/img/other/bacon.jpg)

Roles are created and managed in `Configure » Users`, assuming you have necessary permissions to access it.

# Groups

Just in case you have a complex publishing workflow or lots of cooks in the kitchen (metaphorically speaking of course - if you have lots of real cooks we'd like to try what you're cooking and formally request invites to this event too), assigning multiple Permissions to every User can _also_ be tedious, as are an over abundance of metaphors and subtexts.

Enter: **User Groups**. When a User registers on your site, or you create a user and invite them, you can isolate them to a specific group. Each group has its own assigned Roles, which makes the entire Role assigning process hands-off and automated.

Groups are created and managed in `Configure » Users`, again assuming you have the necessary permissions to access it.

# Conclusion

To explain this any further would likely be futile. Log into the Control Panel, head to the `Configure` area, and start clicking. We're pretty sure you can take it from here.

[front_end_login]: /docs/tags/user-login_form
