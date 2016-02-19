title: Assets
overview: For dealing with all things Asset-related.
class: 'Statamic\API\Assets'
methods:
  -
    name: containers
    description: |
      Get all the Asset Containers.

      ```
      $containers = Assets::containers();
      ```
  -
    name: container
    description: |
      Get an asset container by its ID.

      ```
      $container = Assets::container('b6645a40-9451-11e5-a837-0800200c9a66');
      ```
    signature: $id
id: 65d00f75-c170-4f02-84b4-b455366126eb
