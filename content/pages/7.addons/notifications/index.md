---
title: Flash Notifications
overview: >
  How to trigger flash notifications and messages.
id: b84507d3-ffc5-4443-862b-1d4888ae2025
---
## Via JavaScript

Within your Control Panel Vue components, you may trigger any number of flash notifications at any point.

### Success

Trigger a success notification.

``` .language-js
this.$notify.success('Thing updated!');
```

### Error

Trigger an error notification.

``` .language-js
this.$notify.error('Oh no! That thing failed.');
```

### Options

Both the `success` and `error` methods accept an object as the second argument containing options.

#### Non-dismissible

This will remove the close button.

``` .language-js
this.$notify.error('Oops, please check the values.', { dismissible: false });
```

#### Timeout

This will automatically close the notification after the specified milliseconds.

``` .language-js
this.$notify.success('Updated the thing.', { timeout: 2000 });
```

## Via PHP with Laravel
By returning a controller response with `$success` or `$errors` variables, you can display success error and messages, respectively.

### Success
Trigger a success notification.
```.language-php
return view()
        ->withSuccess('The thing did the thing it was supposed to, hurray!');
```

### Error
Show persistant error messages.
```.language-php
 return redirect()
         ->back()
         ->withErrors(['Oh no it did not do the thing!']);
```
