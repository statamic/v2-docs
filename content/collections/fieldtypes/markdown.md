title: Markdown
overview: Write and preview Markdown with the help of formatting buttons and other neat things.
image: /assets/fieldtypes/markdown.png
id: 607cfe62-7239-461b-8f55-8e7a312c2d5d
options:
  -
    name: container
    type: string
    description: >
      An asset container ID. When specified, the fieldtype will allow the user to add assets from the specified container.
  -
    name: folder
    type: string
    description: >
      The folder (relative to the asset container) to use when choosing assets. If left blank, the root folder of the container will be used.
  -
    name: restrict_assets
    type: bool
    description: >
      If set to `true`, navigation within the asset browser dialog will be disabled, and you
      will be restricted to the container and folder specified.