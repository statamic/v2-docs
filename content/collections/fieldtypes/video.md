---
title: Video
description: Preview videos YouTube, Vimeo, and more just by entering URLs.
overview: |
  Preview videos YouTube, Vimeo, and more just by entering URLs.
image: /assets/fieldtypes/video.png
added_in: 2.8
id: ced8b901-95bd-4006-b70e-4ea04d72fcb7
---
## Usage {#usage}

Type or paste a URL to a video, and it will be displayed beneath so you can preview it.

You may enter:

- YouTube URLs. eg. `https://www.youtube.com/watch?v=s9F5fhJQo34`
- Vimeo URLs. eg. `https://vimeo.com/22439234`
- mp4, ogv, mov, or webm direct URLs. eg. `http://example.com/video.mp4`

You can enter anything else you want, although no video preview will appear.

## Data Structure {#data-structure}

The Video field will simply save whatever you've entered.

``` yaml
video: https://www.youtube.com/watch?v=s9F5fhJQo34
```

## Templating {#templating}

Since the field saves regular Video URLs, you can use the [is_embeddable](/modifiers/is_embeddable) and
[embed_url](/modifiers/embed_url) modifiers to output the appropriate URLs.

```
{{ if video | is_embeddable }}
    <!-- Youtube and Video -->
    <iframe src="{{ video | embed_url }}" ...></iframe>
{{ else }}
    <!-- The rest -->
    <video src="{{ video | embed_url }}" ...></video>
{{ /if }}
```
