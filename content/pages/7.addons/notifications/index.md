---
title: Flash Notifications
overview: >
  Trigger notifications via Javascript.
id: b84507d3-ffc5-4443-862b-1d4888ae2025
---
Within your Control Panel Vue components, you may trigger any number of flash notifications at any point.

## Success

Trigger a green success notification.

``` .language-js
this.$notify.success('Thing updated!');
```

## Error

Trigger a red error notification.

``` .language-js
this.$notify.error('Oh no! That thing failed.');
```

## Options

Both the `success` and `error` methods accept an object as the second argument containing options.

### Non-dismissible

This will remove the close button.

``` .language-js
this.$notify.error('Oops, please check the values.', { dismissible: false });
```

### Timeout

This will automatically close the notification after the specified milliseconds.

``` .language-js
this.$notify.success('Updated the thing.', { timeout: 2000 });
```
