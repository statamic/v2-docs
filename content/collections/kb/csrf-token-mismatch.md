---
title: "TokenMismatchException and Load Balancers"
kb_categories:
  - f470efec-cdc6-4e6a-ac40-bf30f3515cc4
id: 6e1646e0-b1a6-11e6-9598-0800200c9a66
---
If you're getting a `TokenMismatchException` when submitting forms and you're also running a load balancer, this might clear things up.

By default, sessions are stored on the filesystem in `local/storage/framework/sessions`. CSRF tokens are stored in the user's session.

If a user has their session file created on one server, then submits a form which is handled by another server, 
their session may be re-created and result in a missing or invalid token. That's when the `TokenMismatchException` 
gets thrown.

Some potential solutions to this:

- Somehow make sure `local/storage/framework/session` is shared across the servers.
- Change the session driver to something persistent across servers by changing `SESSION_DRIVER` in your `.env` file. Perhaps `memcached` or `redis`.
- Disable CSRF verification by adding `*` to the `csrf_exclude` array in `site/settings/system.yaml`. For obvious reasons, this may be a bad idea.
- Consider if a load balancer is really necessary for your site.
