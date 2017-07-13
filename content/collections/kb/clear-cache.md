---
title: How do I clear the cache?
kb_categories:
  - Tips, Tricks, and How-Tos
id: fa5809bf-79e4-48cc-906f-625214391eaa
---
If you have command line access, the easiest way to clear the cache is to use `please` command.

``` .language-bash
php please clear:cache
```

If that's not working for some reason, and you are using the File cache driver (if you don't know what that means, then
you are), you can manually delete the contents of `local/storage/framework/cache`. This is where Laravel stores any cached
items. Clear this folder out, and the cache is gone.

You can also clear the cache from the Control Panel by going to any settings area and hitting save.
