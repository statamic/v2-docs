---
title: Data Factories
id: 838ac147-3113-43a8-b988-a347a03cb93b
overview: >
  When you need to create a new data object, you shouldn't be "new-ing" it up like: `new Page`.
  Data Factories exist to make your life easier.
---
Statamic has some data factories that provide you with a nice fluent interface for creating them, as well as handling some additional behind-the-scenes logic automatically for you.

Each data type will have its own factory, but they all follow the same basic workflow. First, use their respective API class to start up the factory. Then, you can go ahead and chain methods onto it. Once you're happy, you can get the object out of the factory.

Here's what that might look like:

```
use Statamic\API\Entry;

Entry::create('my-post')
    ->collection('blog')
    ->with(['title' => 'My First Post'])
    ->published(false)
    ->date('2016-05-10')
    ->get();

// Returns a Statamic\Data\Entries\Entry
```

Of course, you don't have to chain everything in one go:

```
$factory = Entry::create('my-post')
                ->collection('blog')
                ->with(['title' => 'My First Post'])
                ->date('2016-05-10');
if ($draft) {
    $factory->published(false);
}

$entry = $factory->get();
```

Once you have your object (an `Entry`, in this case) - remember that it doesn't exist as a file until you save it. That's simple though:

```
$entry->save();
```

Each factory starts off a little different. Entries use `Entry::create($slug)`, Pages use `Page::create($uri)`. Check out the docs for the factory you're using.

- [ContentFactory](/addons/api/contentfactory)
- [PageFactory](/addons/api/pagefactory)
- [EntryFactory](/addons/api/entryfactory)
- [TermFactory](/addons/api/termfactory)
- [GlobalFactory](/addons/api/globalfactory)
