---
title: My license belongs to another domain
kb_categories:
  - Troubleshooting Common Scenarios
id: 33984e54-475e-4a19-86af-05a0abe00192
---
You've logged into the control panel and you're being told that your license belongs to another domain.

You changed it. Great! But the message is still there.

Just [clear your cache](/knowledge-base/clear-cache).

Statamic only "calls home" once an hour and caches the response to prevent your site slowing down more than it needs to.
When you update the domain in your Statamic license area, your Statamic site doesn't know anything changed. Clearing
your cache will force your site to "call home" again, where it will notice your domain change.
