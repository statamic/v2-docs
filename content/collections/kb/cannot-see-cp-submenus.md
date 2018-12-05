---
title: Control panel submenus not showing
kb_categories:
  - Troubleshooting Common Scenarios
id: e86f3460-d6ca-11e8-b568-0800200c9a66
---
Are the submenus under collections and users not showing in the CP? If your site is using https, it can cause issues if your environment is expecting http. You can fix this in by changing your site URL in `system.yaml`:
```
locales:
en:
  full: en_GB
  name: English
  url: "https://example.com"
  ```

  Or `APP_URL=https://example.com`  in your `.env`
