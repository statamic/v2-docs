---
title: Testing
id: f2729cd3-e2fe-4719-bfae-638d33a8b3a3
---

## Creating Tests {#creating-tests}

Statamic will recognize any PHPUnit test classes (which are just PHP classes ending in `Test.php`) located in the 
`site/tests` or `site/addons` directories.

``` .lang-files
/ /
|-- addons/
|   `-- MyAddon/
|       `-- SomeTest.php
|-- site/
|   `-- tests/
|       `-- MyTest.php
`-- statamic/  
```

Your test classes can extend `Statamic\Testing\TestCase` if you need to boot up Statamic, or simply use `PHPUnit_Framework_TestCase` for a true unit test class. Your classes do not need to be namespaced.

## Running Tests {#running-tests}

You may run your tests using the following command:

``` .lang-bash
php please test
```

Any additional arguments will be passed directly onto PHPUnit.  
For example, `php please test --group=stuff`.

Running the command above will run all site and addon tests. To run only your site or addon test suites, you may use the following commands, respectively.

``` .lang-bash
php please test:site
php please test:addons
```

In order for the individual test suites to run, you should verify that your `phpunit.xml` contains two `testsuite` nodes, named `site` and `addons`.

``` .lang-xml
<testsuites>
    <testsuite name="site">
        <directory>./site/tests/</directory>
    </testsuite>
    <testsuite name="addons">
        <directory>./site/addons/</directory>
    </testsuite>
</testsuites>
```


## Dev Dependencies {#dev-dependencies}

Statamic ships _without_ dev dependencies. You will need to have them installed in order to run tests.

To install them, run `composer install` inside the `statamic` directory.

If you check your `statamic` directory into version control, you may wish to discard the changes in there since it will
add a lot of extra files to your project.
