---
title: File
class: Statamic\API\File
id: 0a52b48d-685b-4183-91c9-042c092b3ff3
---
This class lets you manipulate files on the [Statamic's various filesystems](/knowledge-base/filesystems).

## Usage

The way you'd use this class is generally to specify the disk you want to target, then the method.

If you look at the class itself, you will only see two methods and wonder where everything else is. The bulk of the
file methods are located in `Statamic\Filesystem\FileAccessor`.

However, you shouldn't access that class directly. Instead, use the `disk()` method to generate a new instance for you:

```
// Get a page from the "content" filesystem.
// By default this is located in site/content/
File::disk('content')->get('pages/1.about/index.md');
```

Alternatively, if you call methods directly on the API class, the default disk will be used.

```
// The default disk accepts paths relative to the root of your website.
File::get('site/content/pages/1.about/index.md');
```

Keep in mind that the filesystems are designed to be moved around. If you intend on manipulating files that are supposed
to be in one disk – like "content" – you should use that disk instead of the default.

## Methods

### disk

Get an instance of `FileAccessor`, constructed with the specified filesystem disk. You may chain an additional method onto this.

```
$disk = File::disk('content'); // Returns FileAccessor
$disk->put('myfile.txt', 'contents');

// or simply...
File::disk('content')->put('myfile.txt', 'contents');
```

### filesystem

Get the underlying Laravel filesystem adapter.

```
File::filesystem(); // Returns \Illuminate\Contracts\Filesystem\Filesystem
```

### get

Get a file's contents, with a fallback value if the file doesn't exist.

```
File::get($file, $fallback = null); // Returns a string
```

### exists

Check if a file exists.

```
File::exists($file); // Returns a boolean
```

### put

Put content into a file. It will either create a new file or overwrite an existing one.

```
File::put($file, $contents); // Returns a boolean
```

### copy

Copy a file from one location to another. Accepts a boolean third argument which specifies if the destination should
be overwritten if it already exists. By default, it won't.

```
File::copy($src, $dest, $overwrite = false); // Returns a boolean
```

### delete

Delete a file.

```
File::delete($file); // Returns a boolean
```

### rename

Rename a file.

```
File::rename($old, $new);
```

### mimeType

Get the MIME type of a file.

```
File::mimeType($path); // Returns a string
```

### lastModified

Get the last modified timestamp.

```
File::lastModified($file); // Returns an integer
```

### size

Get the size of the file, in bytes.

```
File::size($file); // Returns a integer
```

### sizeHuman

Get the size of the file, in human readable terms.

```
File::sizeHuman($file); // Returns a string, eg. "1 GB", "25 KB", "12 bytes", etc
```

### extension

Get the file extension from a file path. This method doesn't care which disk you use, and the file doesn't have
to actually exist.

```
File::extension('foo.txt'); // Returns "txt"
```

### isImage

Determine if a file is an image. This method doesn't care which disk you use, and the file doesn't have
to actually exist. It checks if the extension is `jpg`, `jpeg`, `png`, or `gif`.

```
File::isImage($file); // Returns a boolean
```
