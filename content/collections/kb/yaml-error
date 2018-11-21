---
title: "YAML Error"
kb_categories:
  - Troubleshooting Common Scenarios
id: 7300a8ee-0d0e-491c-bb39-b1ebf26eaf58
---
If you're getting a `Array to string` error when saving/creating content:
```
ErrorException: Array to string conversion in /home/site/statamic/core/API/YAML.php:68
```

Check to see if you have any `content` fields that aren't just strings, like `Grid`, `Replicator`, `Bard`.

`content` is a reserved word and Statamic assumes it only has textual data, so you'll have to rename your existing field(s) to something else.
