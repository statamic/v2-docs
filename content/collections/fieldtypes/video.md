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

Enter a video URL and it will be loaded and displayed in a player directly beneath so you can preview it. Or watch the whole thing instead of working.

You may enter:

- YouTube URLs: `https://www.youtube.com/watch?v=s9F5fhJQo34`
- Vimeo URLs: `https://vimeo.com/22439234`
- mp4, ogv, mov, or webm direct URLs: `http://example.com/video.mp4`

You can enter anything else you want -- although no video preview will appear. One might be tempted to ask why you would do that, but we refuse to judge.

## Data Structure {#data-structure}

The Video field will save the URL of the video you've entered. If you paste embed code into the field, it will extract the proper URL for you.

``` yaml
video: https://www.youtube.com/watch?v=s9F5fhJQo34
```

## Templating {#templating}

Since the field saves regular Video URLs, you can use the [is_embeddable](/modifiers/is_embeddable) and
[embed_url](/modifiers/embed_url) modifiers to display your video player.

```
{{ if video | is_embeddable }}
    <!-- Youtube and Video -->
    <iframe src="{{ video | embed_url }}" ...></iframe>
{{ else }}
    <!-- The rest -->
    <video src="{{ video | embed_url }}" ...></video>
{{ /if }}
```
