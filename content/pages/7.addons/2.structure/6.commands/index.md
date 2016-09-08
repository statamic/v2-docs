---
overview: "Sometimes people might prefer getting things done through the terminal console. Sometimes it's just easier and faster that way."
title: Commands
id: 58770216-49bf-442f-a0d6-cdfe30a56a08
---
[Read more about console commands][commands].

An addon can have any number of console commands. To create a command, you'll need to create a class file. There's a sample below.
The command classes can go anywhere inside your addon's folder, but we recommend putting them inside a `Commands` subdirectory.
The classes should be named `[AddonName][CommandName]Command.php`, eg. `HelloGreetCommand.php`.

You can even use a command to generate a command file. (How meta!) `php please make:command AddonName` will generate one for you.

## Example

The following is a simple example command belonging to the `Hello` addon.

* You can type `php please hello:greet` to get greeted. How nice!
* There's a `name` argument that lets you customize the output. (Since a default value of `World` has been set, it makes it optional.)
* There's a `shout` option that will add some exclamation points.

``` .language-php
<?php

namespace Statamic\Addons\Hello\Commands;

use Statamic\Extend\Command;

class HelloGreetCommand extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'hello:greet
                            {name=World : The name to be greeted}
                            {--shout= : Whether to shout or not}';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Say hello!';

    /**
     * Create a new command instance.
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return mixed
     */
    public function handle()
    {
        $message = 'Hello ' . $this->argument('name');

        if ($this->option('shout')) {
            $message .= '!!!';
        }

        $this->info($message);
    }
}
```

Everything you see here comes from Laravel, there's not really anything specific to Statamic.
For more details, consult the [Laravel docs][laravel-artisan] for how to write your commands.

However, here's an overview:

* The `$signature` dictates your command's syntax. (You can name it whatever you like, but we recommend you prefix it with
your addon's name to prevent conflicts.) You may also define arguments and options.
* The `$description` will be shown in the command list when you run `php please show`. It - as you'd expect - should
describe what your command does.
* The constructor allows you to typehint any dependencies from the service container. This isn't required.
* The `handle` method is what will happen when your command is run.

In addition to any methods and properties you might expect in a Laravel command class, you can also use everything
inherited from Statamic's `Addon` class.


[commands]: /
[laravel-artisan]: http://laravel.com/docs/5.1/artisan#writing-commands