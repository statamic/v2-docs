---
title: Update All Collection Entries
overview: >
  Sometimes it's necessary to apply fieldset changes to all entries of a collection. Here's how.
---

## Update All Collection Entries

Sometimes it's necessary to apply fieldset changes to all entries of a collection. One such scenario is adding a toggle fieldset with a default value of true. The value does not get set by default, only when the entry is next saved via the control panel. How to do this?

### How? Use `php please tinker`

The Laravel-based `tinker` command-line mode essentially allows you to run arbitrary php commands on your site. This means we can use it to update all the entries of a collection.

Here are the steps, followed by their details:

1. Enter `tinker` mode
2. paste/type php code that:
  1. loops through the specific collection needing to be updated
  2. updates the entry as needed
  3. saves the entry

#### 1. Enter `tinker` mode
To enter `tinker` mode open a terminal, go to the root of your Statamic install and run `php please tinker`.

#### 2. Paste/type php code

Here's sample code that updates a site's `blog` collection and adds or updates the `comments-on` field to `true`: 

```
\Statamic\API\Entries::getFromCollection('blog')->each(function ($entry) {
    $entry->set('comments-on', true);
    $entry->save();
});
```

Done! Much easier than opening all those pages in the control panel and clicking "save," right?

#### Why not just `$entry->save();`?

You may be wondering why it's not possible to simply run `$entry->save();` rather than explicity having to perform each desired action (ie: programmatically having to perform the desired updates). After all, cliking "save" in the control panel does update all the fieldsets.

The reason: The control panel has all the fields loaded so clicking "save" naturally loads updates them to the lateset.

### Exiting `tinker` mode

To exit `tinker` mode click <kbd>ctrl/cmd</kbd>+<kbd>c</kbd>
