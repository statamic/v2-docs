---
title: How can I authenticate users with OAuth?
kb_categories:
  - 41f8eb46-342c-4a52-91a3-809d971a5165
id: 1fd12dea-5207-44ce-b654-d57aff7cd9a4
---

As of Statamic 2.1, authenticating with OAuth is supported with the help of Laravel Socialite and a simple bridge addon
that connects the pieces together.

Out of the box, Statamic supports the providers that are bundled with Socialite (Facebook, Twitter, Google, LinkedIn, GitHub, and
Bitbucket).

The [Socialite Providers][socialite_providers] Github organization contains a large number of
additional providers (91 at the time of writing) that require minimal effort to install.

Finally, if you require a provider that isn't on that list, you may write your own, which is also quite straight forward.
[Here's how.](#custom-providers)

## Setup

Install the OAuth Bridge addon files. You can find them below.

If using any third party providers from [SocialiteProviders](http://socialiteproviders.github.io), add their composer package to the `require` array in OAuthBridge's `composer.json` file. These are normally in the format `"socialiteproviders/dropbox": "^2.0"`. Then run `php please addons:refresh` to install it.

Add the providers to the `OAuthBridgeServiceProvider`'s `$oauth_providers` array with the name of the provider as the key, and the event listener as the value. If you are using a provider that comes with Socialite (Facebook, Twitter, Google, LinkedIn, GitHub and Bitbucket), the listener can be `null`.


### OAuthBridge addon files

`site/addons/OAuthBridge/composer.json`

``` .language-php
{
    "name": "statamic/oauth-bridge",
    "require": {
    }
}

```

`site/addons/OAuthBridge/OAuthBridgeServiceProvider.php`

``` .language-php
<?php

namespace Statamic\Addons\OAuthBridge;

use Statamic\Providers\OAuthServiceProvider as ServiceProvider;

class OAuthBridgeServiceProvider extends ServiceProvider
{
    /**
     * An array of OAuth providers and their listeners
     *
     * @var array
     */
    protected $oauth_providers = [
        // Built-in providers can be null.
        // 'facebook' => null,

        // SocialiteProviders use the event class documented on their site.
        // 'dropbox' => 'SocialiteProviders\Dropbox\DropboxExtendSocialite'
    ];
}
```


## Configuring OAuth

### Create an application

Create an OAuth application on the service of your choice. Take note of the client and secret keys.

When they ask for a "callback URL" or "redirect URL", you should provide `http://your-site.com/oauth/{provider}/callback`, where `{provider}` would be `github`, `facebook`, etc.

### Add your credentials to Statamic

The Socialite docs will instruct you to add your credentials to `services.php`. Instead, add your OAuth credentials to `site/settings/services.yaml` in the format of:

``` .language-yaml
dropbox:
  client_id:
  client_secret:
github:
  client_id:
  client_secret:
```

Statamic will put these in the right place behind the scenes.

You can use environment variables here by doing `client_id: '{env:GITHUB_CLIENT_ID}'`, etc.

Other Socialite documentation will normally instruct you to add `redirect` keys to each service. You can leave these out, Statamic will automatically set them to `http://your-site.com/oauth/{provider}/callback`. for you

### Routing

Out of the box, the oauth route will be located at `/oauth/*`. You can change that by adding the following to your `OAuthBridgeServiceProvider`:

``` .language-php
public static $oauth_route = 'foo/bar';
```

This will change the routes to `/foo/bar/*`.


## Usage

Send your users to the provider's login URL to begin the OAuth workflow. You may do this with the `oauth` tag.

```
<a href="{{ oauth:login_url provider='github' }}">Log in with Github</a>
```

or the shorthand

```
<a href="{{ oauth:github }}">Log in with Github</a>
```


## Customization

By default, when someone authenticates with a service, Statamic will look for an existing user that is assigned the corresponding OAuth ID, and log in as them. If no matching user is found, a user will be created with an automated username.

### User creation

You may override the whole user creation logic by listening for `Statamic\Events\OAuth\FindingUser`. You may perform whatever logic you need, as long as you return an instance of a `Statamic\Contracts\Users\User`.

If you return anything else, Statamic will continue with its default workflow.

Example:

``` .language-php
public $events = [
    'Statamic\Events\OAuth\FindingUser' => 'findUser'
];

public function findUser(FindingUser $event)
{
    if ($event->provider !== 'provider_we_are_interested_in_handling') {
        return; // Returning nothing will make Statamic continue as per usual.
    }

    // create a user ...

    return $user;
}
```

By listening for this event and providing a user, you are in control of all the logic.


### Username creation

If you don't need to control all the user find/create logic, but would like to customize the username, you may listen for the `Statamic\Events\OAuth\GeneratingUsername` event and then return a string.

Example:

``` .language-php
public $events = [
    'Statamic\Events\OAuth\GeneratingUsername' => 'username'
];

public function username(GeneratingUsername $event)
{
    return 'a-custom-username';
}
```

By default, Statamic will generate a username by slugifying the email address and appending a timestamp.

### User data creation

Like the username creation, you may also just specify the data to be saved to the user.

Listen for the `Statamic\Events\OAuth\GeneratingUserData` event and return an array.

Example:

``` .language-php
public $events = [
    'Statamic\Events\OAuth\GeneratingUserData' => 'userData'
];

public function userData(GeneratingUserData $event)
{
    return ['foo' => 'bar'];
}
```

## Custom Providers {#custom-providers}

If your OAuth provider isn't already available, you may create your own provider.

Before you dive into creating your own, you should check the [SocialiteProviders][socialite_providers] site, there's
close to 100 providers ready for you to drop in.

To create a provider, you'll basically be following the documentation on the [SocialiteProviders Contribute page](http://socialiteproviders.github.io/#contribute) with a couple of things relocated for Statamic.

We'll be creating a provider that authenticates with the Bacon service. Wouldn't it be nice to authenticate with bacon?

First up, you'll need to create the `Provider` class as per their docs. This is the class that Socialite uses to
communicate with the OAuth server. Here's a little boilerplate:

`site/addons/BaconOAuth/Provider.php`

``` .language-php
<?php
namespace Statamic\Addons\BaconOAuth;

use Laravel\Socialite\Two\ProviderInterface;
use SocialiteProviders\Manager\OAuth2\AbstractProvider;

class Provider extends AbstractProvider implements ProviderInterface
{
    // implement the methods from the provider interface
}
```

Next is the event handler. Again, it's just like the examples from their docs. Make sure you point to wherever you placed
the `Provider.php` class.

`site/addons/BaconOAuth/BaconExtendSocialite.php`

``` .language-php
<?php

namespace Statamic\Addons\BaconOAuth;

use SocialiteProviders\Manager\SocialiteWasCalled;

class BaconExtendSocialite
{
    /**
     * Register the provider.
     *
     * @param \SocialiteProviders\Manager\SocialiteWasCalled $socialiteWasCalled
     */
    public function handle(SocialiteWasCalled $socialiteWasCalled)
    {
        $socialiteWasCalled->extendSocialite(
            'bacon',  // the key you will use in the event listener
            'Statamic\Addons\BaconOAuth\Provider'
        );
    }
}
```

That's it! Make sure to add your provider to the Bridge addon's `$oauth_providers` array. You'll use the key you
defined in the event handler class, and the event will be the event handler's class.

``` .language-php
protected $oauth_providers = [
   'bacon' => 'Statamic\Addons\BaconOAuth\BaconExtendSocialite'
];
```

[socialite_providers]: https://socialiteproviders.github.io/
