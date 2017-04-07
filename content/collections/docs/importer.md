---
title: Importer
overview: >
  The importer allows you to bring across data from an existing site without needing to recreate
  everything by hand.
id: 259e6934-dd57-4860-b4b6-2eeafd1de187
---

## How it works {#how-it-works}

The Importer can be found in the Control Panel under the "Tools" section.

You upload a JSON file in the [appropriate format](#creating-your-own-exporter), then customize what you want imported.

The importer will let you adjust routes, manage duplicate content, and specify which data you want imported. Once you're 
happy, click import and all the files will be created automatically for you.

## Importing from Statamic v1 {#importing-from-statamic-v1}

We've provided an companion exporter that will generate the appropriate JSON file for your Statamic v1 sites.

You can download it [on Github][exporter]. Once you have the JSON file, import it as described above.

## Creating your own exporter {#creating-your-own-exporter}

In your previous CMS or app of choice, have it generate a JSON in the following schema.

``` .language-js
{
    "collections": {
        "posts": {
            "\/posts\/example-post": {
                "order": "2016-05-24",
                "data": {
                    "title": "An Example post",
                    "content": "This is some post content",
                    "categories": ["news", "updates"],
                    "tags": ["bacon"],
                    "other_fields": "can go here",
                    ...
                }
            },
            "\/posts\/another-post": { ... },
            ...
        },
        "events": {
            "\/events\/event-one": { ... },
            "\/events\/event-two": { ... },
            ...
        },
        ...
    },
    "pages": {
        "\/sample-page": {
            "order": 1,
            "data": {
                "title": "Sample Page",
                "content": "The page content",
                "other_fields": "can go here",
                ...
            }
        },
        "\/another-page": { ... },
        ...
    },
    "taxonomies": {
        "categories": {
            "news": {
                "title": "News",
                "foo": "bar"
            },
            "updates": { ... },
            ...
        },
        "tags": {
            "bacon": { ... }
        },
        ...
    }
}
```

### Things to note {#export-notes}

Since taxonomy terms can be automatically generated from content, if you aren't storing any additional fields in your
terms, you can include empty arrays for the taxonomies. That way, unnecessary files won't be generated. For example:

``` .language-js
"taxonomies": {
    "categories": [],
    "tags": []
}
```

[exporter]: http://github.com/statamic/exporter