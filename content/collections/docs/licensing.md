---
title: Licensing
id: 9e838cce-abbc-49a4-8d9a-6394a4dcd767
overview: Statamic is **commercial software**, so we have a few terms and rules to go over. These are pretty important. Study up!
---

In a nutshell, you can use Statamic **locally** without a license key. this is  called [Trial Mode][trial-mode]. Once it's time to launch the site on  a _public_ domain, you need to buy a license and add the key to the settings area of the Control Panel (or in `site/settings/system.yaml`).

## How the license security works {#technical}

If you want to know about the _legal terms_ you can [read those here][terms].
The rest of this article covers the technical aspects of the call-home features, domain restrictions, and so forth.

## Statamic needs to "phone home"... {#outpost}

![Phone Home](https://media.giphy.com/media/qI5hzHmRb7Dr2/giphy.gif) {.rounded-lg .w-full .mt-1}

Statamic will ping The Outpost (our wilderness-branded web service) on a regular basis. The Outpost collects the license key and public domain info (Domain Name, IP Address, etc) so we can validate them against your account.

This happens just once an hour, when logged into the Control Panel. Changing your license key setting will trigger an immediate ping to the The Outpost. Tampering with outgoing API call will cause Statamic to consider your license invalid. We might also send our Flying Enforcer Monkeys™ to your office to throw unmentionable things at you. We might not. But we might.

## ...because every website needs a license. {#one-domain}
A Statamic license entitles you to use it on _one domain_. You will need to specify the domain you plan to use from the [license area of your Statamic Account][account]. This domain will be treated as a wildcard so you can use subdomains for locales, testing, and other purposes.

This also means that you can only use the Control Panel from your _default_ locale.

If you attempt to use the site from another domain you will get a notification inside the Control Panel informing you that your key is being used on more than one site and be prompted to make the necessary changes. You may [change the domain][account] associated with a license at any time.

## What is a public domain? {#public-domain}

When Statamic calls home we use a series of rules to determine if the domain it's running on is considered "public".

If any of the following rules match the domain is considered **not public** (letting you stay in Dev Mode)

- Is it a single segment? eg. `localhost`
- Is it an IP address?
- Does it use a port other than `80` or `443`?
- Does it have a dev-related subdomain? `test.`, `testing.`, `sandbox.`, `local.`, `stage.`, or `staging.`
- Does it use a dev-related TLD? `.dev`, `.local`, `.localhost`, `.test`, `.invalid`, `.example` or `.app`

## Special Circumstances {#special}

We understand that sometimes your domains can't be set up to coincide with our rules exactly because of firewall or client needs, backwards compatibility, SEO, or the humidity level in Denver. We totally get it, and have a solution.

You can comma delimit domains in your licenses page, like so: `mysite.com, mystagingsite.com`. These will be reviewed periodically to ensure they're not being abused to get around the One License One Website™ rule. But you wouldn't do that to a little small business, would you?

[trial-mode]: /knowledge-base/trial-mode
[terms]: https://statamic.com/terms
[account]: https://account.statamic.com/licenses
