---
title: Asset
class: 'Statamic\API\Asset'
overview: Create or fetch single assets.
methods:
  -
    name: create
    description: |
      Kickstart the Asset creation process. This returns an `AssetFactory` instance, and will allow you to continue chaining methods to build your Asset.

      ```
      $asset = Asset::create()
                    ->container('ac0ea4c0-9455-11e5-a837-0800200c9a66')
                    ->folder('img')
                    ->get();
      ```
    signature: $id = null
  -
    name: get
    description: |
      Get an existing `Asset` object by its ID.

      ```
      $asset = Asset::get('bf75af40-9455-11e5-a837-0800200c9a66');
      ```
    signature: $uuid, $locale = null
id: 3c290e24-113a-4e8c-b39c-a7b747e32262
---
## Multiple Assets

This class is for fetching single assets - it's just nicer to read `Asset` instead of `Assets` in your code if you're only expecting a single asset.

To deal with multiple assets, see the [Assets class](/addons/apis/assets).
