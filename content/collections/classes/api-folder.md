---
title: Folder
class: Statamic\API\Folder
id: 1e821933-4037-4a18-8529-fbb2aba70ade
---
This class lets you manipulate folders on the [Statamic's various filesystems](/knowledge-base/filesystems).

## Usage

The way you'd use this class is generally to specify the disk you want to target, then the method.

If you look at the class itself, you will only see two methods and wonder where everything else is. The bulk of the
folder methods are located in `Statamic\Filesystem\FolderAccessor`.

However, you shouldn't access that class directly. Instead, use the `disk()` method to generate a new instance for you:

```
// Get a page from the "content" filesystem.
// By default this is located in site/content/
Folder::disk('content')->exists('pages/about');
```

Alternatively, if you call methods directly on the API class, the default disk will be used.

```
// The default disk accepts paths relative to the root of your website.
Folder::exists('site/content/pages/about');
```

Keep in mind that the filesystems are designed to be moved around. If you intend on manipulating files and folders that
are supposed to be in one disk – like "content" – you should use that disk instead of the default.

## Methods

### disk

Get an instance of `FolderAccessor`, constructed with the specified filesystem disk. You may chain an additional method onto this.

```
$disk = Folder::disk('content'); // Returns FolderAccessor
$disk->exists('myfolder');

// or simply...
Folder::disk('content')->exists('myfolder');
```

### filesystem

Get the underlying Laravel filesystem adapter.

```
Folder::filesystem(); // Returns \Illuminate\Contracts\Filesystem\Filesystem
```

### exists

Check if a folder exists.

```
Folder::exists($folder); // Returns a boolean
```

### make

Make a directory.

```
Folder::make($folder); // Returns a boolean
```

### getFiles

Get the file paths inside a folder. The second argument is a boolean for whether to look recursively.

```
Folder::getFiles($folder, $recursive = false); // Returns an array
```

### getFilesRecursively

Get the file paths inside a folder, recursively. This is an alias to `getFiles` with the recursive flag true.

```
Folder::getFilesRecursively($folder); // Returns an array
```

### getFilesByType

Get the file paths inside a folder and filter them by type/extension. The third argument is a boolean for whether to look recursively.

```
Folder::getFilesByType($folder, $extension, $recursive = false); // Returns an array
```

### getFilesByTypeRecursively

Get the file paths inside a folder, recursively, and filter them by type/extension. This is an alias to `getFilesByType`
with the recursive flag true.

```
Folder::getFilesByTypeRecursively($folder, $extension); // Returns an array
```

### getFolders

Get the paths of all folders inside a folder. The second argument is a boolean for whether to look recursively.

```
Folder::getFolders($folder, $recursively = false); // Returns an array
```

### getFoldersRecursively

Get the paths of all folders inside a folder, recursively. This is an alias to `getFolders` with the recursive flag true.

```
Folder::getFoldersRecursively($folder); // Returns an array
```

### lastModified

Get the last modified timestamp.

```
Folder::lastModified($folder); // Returns an integer
```

### isEmpty

Check if a folder is empty.

```
Folder::isEmpty($folder); // Returns a boolean
```

### copy

Copy a folder from one location to another. Accepts a boolean third argument which specifies if the destination should
be overwritten if it already exists. By default, it won't.

```
Folder::copy($src, $dest);
```

### rename

Rename a folder.

```
Folder::rename($old, $new);
```

### delete

Delete a folder.

```
Folder::delete($folder); // Returns a boolean
```

### deleteEmptySubfolders

Delete any empty subfolders, recursively.

```
Folder::deleteEmptySubfolders($folder);
```
