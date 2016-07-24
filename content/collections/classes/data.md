---
class: Statamic\Data\Data
title: Data
id: 8e4e9077-5393-44c4-b26f-fd18831c9e6e
---
The `Data` class is an abstract class that is the basis for most content types. It deals with the manipulation of
data on the object in multiple locales, paths, IDs, saving, deleting, and more.

For the purposes of examples, the following code snippets will be performed using a `$page` (`Statamic\Data\Pages\Page`).

## locale

Get or set the current locale.

```
$page->locale(); // Returns a string. en, fr, etc.

$page->locale('fr'); // Returns null
```

## resetLocale

Reset the locale back to the default. If you had previously changed the locale, this would set it back to the default.

```
$page->resetLocale(); // Returns null
```

## in

Get the object in a specified locale. Returns a `LocalizedData` object.
[Read more about manipulating localized data.](/addons/data)

```
$page->in('fr'); // Returns LocalizedData
```

## locales

Get an array of the locales this object contains.

```
$page->locales(); // Returns array
```

## hasLocale

Check if the object has been localized into a given locale.

```
$page->hasLocale('fr'); // Returns a boolean
```

## removeLocale

Remove data for a locale.

```
$page->removeLocale('fr'); // Returns null
```

## isDefaultLocale

Check if the current locale is the default locale.

```
$page->isDefaultLocale(); // Returns a boolean
```

## data

Get or set all the data for the current locale.

```
$page->data(); // Returns an array

$page->data($data); // Returns null
```

## dataForLocale

Get or set the data for a given locale.

```
$page->dataForLocale('fr'); // Returns an array

$page->dataForLocale('fr', $data); // Returns null
```

## dataWithDefaultLocale

Get the data for the current locale, merged with the default locale data.

```
$page->dataWithDefaultLocale(); // Returns an array
```

## defaultData

Get the data from the default locale.

```
$page->defaultData(); // Returns an array
```

## get

Get a key from the data, _without_ falling back to the cascade.

```
$page->get($key, $default); // returns mixed
```

## getWithCascade

Get a key from the data, and fall back to cascade. Cascading data could be folder.yaml files and the default locale data.

```
$page->getWithCascade($key, $default); // returns mixed
```

## has

Check if the given key exists in the data.

```
$page->has($key); // Returns a boolean
```

## hasWithCascade

Check if the given key exists in the data, including the cascade.

```
$page->hasWithCascade($key); // Returns a boolean
```

## set

Set a key in the data.

```
$page->set($key, $value); // Returns Page
```

## remove

Remove a key from the data.

```
$page->remove($key); // Returns Page
```

## processedData

Get the data after it has been pre-processed by its corresponding fieldset's fieldtypes.

```
$page->processedData(); // Returns an array
```

## syncOriginal

Sync the original object with the current object. Each object keeps a copy of its original state so that its time to
save, it can determine what has changed. Usually there's no need to call this manually as saving will handle it.

```
$page->syncOriginal(); // Returns Page
```

## dataType

Get or set the data type (in other words, the file extension).

```
$page->dataType(); // Returns a string

$page->dataType('md'); // Returns Page
```

## content

Get or set the content. Generally speaking, the _content_ is the data below the front-matter. When _getting_, the data
will also check the cascade.

```
$page->content(); // Returns a string

$page->content($content); // Returns Page
```

## parseContent

Parses the content as their content type (eg. markdown or textile), smartypants, and as an Antlers template.

```
$page->parseContent(); // Returns a string
```

## path

Get or set the path. There's generally no need to set the path manually.

```
$page->path(); // Returns a string
```


## localizedPath

Get the localized path for a given locale.

```
$page->localizedPath($locale); // Returns a string
```

## originalPath

Get the original path, before any modifications to the object.

```
$page->originalPath(); // Returns a string
```

## originalLocalizedPath

Get the original localized path in a given locale, before any modifications to the object.

```
$page->originalLocalizedPath($locale); // Returns a string
```

## id

Get or set the ID

```
$page->id(); // Returns a string

$page->id($id); // Returns Page
```

## ensureId

Ensure that the object has an ID.

The first parameter is a boolean for whether or not to also save the object. By default it will _not_ save.

```
$page->ensureId(); // Returns null
```

## toArray

Convert the object to an array. This is typically how the object will be used in templates.

```
$page->toArray(); // Returns an array
```

## supplements

Get the supplemented data.

When running `toArray()`, the object will be able to generate supplemental data to be added to the array.

```
$page->supplements(); // Returns an array
```

## getSupplement

Get a key from the supplemented data.

```
$page->getSupplement($key, $default); // Returns mixed
```

## setSupplement

Set a key in the supplemented data.

```
$page->setSupplement($key, $value); // Returns Page
```

## removeSupplement

Remove a key from the supplemented data.

```
$page->removeSupplement($key); // Returns Page
```

## save

Save the object.

```
$page->save(); // Returns Page
```

## delete

Delete the object.

```
$page->delete(); // Returns null
```
