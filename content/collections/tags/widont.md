---
title: Widont
parse_content: false
overview: >
  Attempts to prevent widows (a line with a single word) in a string by adding non-breaking spaces
  between the last two words of each paragraph.
description: Prevent widows in your content.
id: c56974d4-70cf-45da-82d9-e25c1f4078a8
---
## Usage {#usage}

```
{{ widont }}
	I Just Want Pretty Headlines and Sentences
{{ /widont }}
```

```.language-output
I Just Want Pretty Headlines and&nbsp;Sentences
```
