---
title: "Images aren't loading."
kb_categories:
  - f470efec-cdc6-4e6a-ac40-bf30f3515cc4
id: a2160bcb-f860-4ee7-b88e-e6bf3bb3faab
---
Trying to resize an image using the [Glide Tag][glide] and it doesn't load? Or, thumbnails aren't appearing in the
control panel? There are a couple of reasons this might be happening.

- **Running out of memory**  
  Resizing images, especially large ones, can be quite memory intensive. You might need to increase your PHP memory
  limit. If you're using Apache, you can usually add `php_value memory_limit 256M` to your `.htaccess` file.

  Try bumping the memory higher than you need it, just to see if it fixes the problem, then lower it as necessary.

- **Invalid asset ID or image path**  
  The [Glide Tag][glide] expects to you to pass through an ID of an asset, or a path to an image file. If either of
  these are incorrect, the image resizing won't happen. Perhaps you aren't passing in the correct ID, or there is
  a typo in the image path.

[glide]: /reference/tags/glide
