---
title: "TokenMismatchException and Load Balancers"
kb_categories:
  - Troubleshooting Common Scenarios
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


## TokenMismatchException in the Control Panel

If AJAX scripts are not loading in the Control Panel (i.e. Suggestion boxes, any time you try and save data), this could be down to a `TokenMismatchException` - you can check this by firing up Inspector and checking the Resources tab. If you have changed your `APP_KEY` (i.e. if you've updated to a new Statamic website) then this could cause a `TokenMismatchException`, try clearing your browser's cookies.
