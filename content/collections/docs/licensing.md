---
title: Licensing
id: 9e838cce-abbc-49a4-8d9a-6394a4dcd767
overview: Statamic is **commercial software**, so there are some terms of use rules to go over so you can be a kind, good-hearted person and help us continue to grow and support Statamic for millenia to come.
---

In a nutshell, you can use Statamic _locally_ without a license key. We call this [Dev Mode][dev-mode]. Once you want to launch the site and put it on a _public_ domain, you'll need to buy a license and add your key to the site. You won't be able to use the Control Panel until you do.

You can provide your *license key* in the _Settings_ section of the _Control Panel_, or in `site/settings/system.yaml` as `license_key`.

## How the license security works {#technical}

If you want to know about the _legal terms_ you can [read those here][terms].
The rest of this article covers the technical aspects of the call-home features, domain restructions, and so forth.

### The Outpost {#outpost}

Statamic will "radio in: to our web service we affectionately named _The Outpost_ because we like to brand things. We collect just the license key and domain so we can validate that they are connected and everything is cool.

This happens just once an hour, and only when logged into the control panel. Changing your license key setting will force Statamic to ping The Outpost immediately. Tampering with outgoing API call will cause Statamic to consider your license invalid. We'll also send our flying police monkeys to your office to throw poop at you. Maybe.

### One License = One Website {#single-website}

A Statamic license entitles you to use it on _one domain_. You need to specify the domain you plan to use from the [licenses area of your Statamic Account][account]. This domain will be treated as a wildcard so you can use subdomains for locales and so on.

This also means that you can only use the Control Panel from your _default_ locale.

If you attempt to use the site from another domain, you'll get a notification inside the Control Panel informing you that your key is being used on more than one site, and give you some time to figure that out. You may [change the domain][account] associated witha license at any time.

### Developer Mode {#developer-mode}

Developer Mode gives you access to all features of Statamic as long as you are on a non-public domain (e.g. your local computer).

[Read more about Developer Mode][dev-mode]

### What is a public domain? {#public-domain}

When Statamic calls home we use a series of rules to determine if the domain it's running on is considered "public".

If any of the following rules match the domain is considered **not public** (letting you stay in Dev Mode)

- Is it a single segment? eg. `localhost`
- Is it an IP address?
- Does it use a port other than `80` or `443`?
- Does it have a dev-sounding subdomain? eg. `test.`, `testing.`, `sandbox.`, `local.`, `stage.`, or `staging.`
- Does it use a dev-related TLD? eg. `.dev`, `.local`, or `.app`

### But you're special, eh? {#special}

If everyone was special, then no one would be. But, we understand that sometimes your domains can't be set up to coincide with our rules exactly because of client needs, backwards compatibility, SEO, or the humidity level in Denver. We totally get it.

For example, maybe your staging environment is on an entirely separate domain, like mysite.com and mystagingsite.com. That's okay. You can comma delimit domains in your licenses page, like so: `mysite.com, mystagingsite.com`. Problem solved.

[dev-mode]: /knowledge-base/developer-mode
[terms]: https://statamic.com/terms
[account]: https://account.statamic.com/licenses
