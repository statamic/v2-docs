---
overview: >
  Statamic is built upon Laravel, which
  makes heavy use of _Service Providers_
  to bootstrap functionality into the
  application.
title: Service Providers
id: 35d4de1b-a602-4b52-9608-0566c0d65e2b
---
An addon may have its own Service Provider. Simply name it `AddonNameServiceProvider.php` and follow the instructions in the [Laravel documentation][providers].

You can also use `php please make:provider AddonName` to generate the boilerplate for you.

[Read more about Service Providers on the Laravel website][providers].

[providers]: http://laravel.com/docs/5.1/providers
