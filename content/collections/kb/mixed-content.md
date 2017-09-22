---
title: "Mixed Content Error"
kb_categories:
  - Troubleshooting Common Scenarios
id: 61f2d69c-9aee-4b02-a7be-3b9cae1a5a2d
---
If you're getting a `Mixed Content` error:
```
Mixed Content: The page at 'https://yoursite.com/cp/collections/entries/stores' was loaded over HTTPS, but requested an insecure XMLHttpRequest endpoint 'http://yoursite/cp/collections/entries/stores/get?sort=title&order=asc&page=1'. This request has been blocked; the content must be served over HTTPS.
```

when accessing your control panel, add `use_https: true` to your `system.yaml`. Also ensure your site url has `https` in it.

If your assets have absolute URLs, `HTTPS=on` should be added to `.env`.
